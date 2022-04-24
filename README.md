# cert-manager

cert-manager auto create and renew secret file

## ClusterIssuer

### [SelfSigned](https://cert-manager.io/docs/configuration/selfsigned/)
### [Ali DNS](https://github.com/pragkent/alidns-webhook)
### [Godaddy DNS](https://github.com/snowdrop/godaddy-webhook)

## Jenkins

Jenkins UI 涉及的环境变量和可选值

| 环境变量    | 值          | 说明                                                         |
|:------------|:------------|:-------------------------------------------------------------|
| BRANCH_NAME |             |                                                              |
| DEPLOY_ENV  | dev/prod    | dev: 开发环境使用自签名证书, prod: 使用 letsencrypt 获取证书 |
| DNS_VENDOR  | ali/godaddy |                                                              |

## 提供的签名机构名称

| ClusterIssuer | Name                      |
|:--------------|:--------------------------|
| selfsign      | selfsigned-cluster-issuer |
| alidns        | alidns-cluster-issuer     |
| godaady       | godaadyns-cluster-issuer  |
