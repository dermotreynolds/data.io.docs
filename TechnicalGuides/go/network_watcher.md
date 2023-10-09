---
sort: 1
---

# Query Network Watcher using go

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"os"

	"github.com/Azure/azure-sdk-for-go/profiles/2020-09-01/network/mgmt/network"
	"github.com/Azure/azure-sdk-for-go/services/operationalinsights/v1/operationalinsights"
	"github.com/Azure/go-autorest/autorest"
	"github.com/Azure/go-autorest/autorest/adal"
)

var (
	activeDirectoryEndpoint   = "https://login.microsoftonline.com/"
	tenantID                  = "<tenant id>"
	clientID                  = "<client id>"
	clientSecret              = "<Put the secret in here>"
	activeDirectoryResourceID = "https://api.loganalytics.io/"
	subscriptionID    = "<subscription id>"
	baseURI           = "https://management.azure.com/"
	resourceGroupName = "existingResourceGroupName"
)

//CreateToken creates a service principal token
func CreateToken() (adal.OAuthTokenProvider, error) {
	var token adal.OAuthTokenProvider
	oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
	token, err = adal.NewServicePrincipalToken(
		*oauthConfig,
		clientID,
		clientSecret,
		activeDirectoryResourceID)
	return token, err
}

func main() {

	workspaceId := "47cd5c1f-6686-40ea-9053-a07a22710dd6"

	outboundConnections := "AzureNetworkAnalytics_CL					" +
		"| where VM1_s == \"rg-idig-d-eun-ecom-001d07acf64/vm-ecom-d-d01c2e353f7\"                          " +
		"| where FlowStatus_s == \"A\"                                  " +
		"| extend DestPublicIPs_s_Array = split(DestPublicIPs_s, \"|\") " +
		"| extend SrcPublicIPs_s_Array = split(SrcPublicIPs_s, \"|\")   " +
		"| extend VM1_s_Array = split(VM1_s, \"/\")                     " +
		"| extend VM2_s_Array = split(VM2_s, \"/\")                     " +
		"| project      SrcVm = tostring(VM1_s_Array[1]),               " +
		"               SrcVmResourceGroup = tostring(VM1_s_Array[0]),  " +
		"               SrcVmIp = SrcIP_s,                              " +
		"               DestVm = tostring(VM2_s_Array[1]),              " +
		"               DestVmResourceGroup = tostring(VM2_s_Array[0]), " +
		"               DestVmIp = DestIP_s,                            " +
		"               SrcPublicIp = tostring(SrcPublicIPs_s_Array[0])," +
		"               DestPublicIp = tostring(DestPublicIPs_s_Array[0]),  " +
		"               DestPort = toint(DestPort_d),                       " +
		"               L4Protocol = L4Protocol_s,                          " +
		"               L7Protocol = L7Protocol_s,                          " +
		"               FlowStatus = FlowStatus_s,                          " +
		"               FlowDirection = FlowDirection_s                     " +
		"| distinct     SrcVm,                                              " +
		"               SrcVmResourceGroup,                                 " +
		"               SrcVmIp,                                            " +
		"               DestVm,                                             " +
		"               DestVmResourceGroup,                                " +
		"               DestVmIp,                                           " +
		"               DestPort,                                           " +
		"               SrcPublicIp,                                        " +
		"               DestPublicIp,                                       " +
		"               L4Protocol,                                         " +
		"               L7Protocol,                                         " +
		"               FlowStatus,                                         " +
		"               FlowDirection                                       "

	token, _ := CreateToken()
	vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
	vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
	qryclient := operationalinsights.NewQueryClient()
	qryclient.Authorizer = autorest.NewBearerAuthorizer(token)

	var qbdy operationalinsights.QueryBody

	qbdy.Query = &outboundConnections

	qbdy.Workspaces = &[]string{"la-sub-idig-d-001"}

	// fmt.Println(qbdy.Workspaces)

	response, err := qryclient.Execute(context.Background(), workspaceId, qbdy)

	if err != nil {
		fmt.Printf("%v\n", err)
		os.Exit(1)
	}

	b, err := json.MarshalIndent(response, "", "\t")
	if err != nil {
		fmt.Printf("Error: %s", err)
	}
	fmt.Println(string(b))

}
```
