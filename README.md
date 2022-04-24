# cert-manager

cert-manager auto create and renew secret file

## ClusterIssuer

### [SelfSigned](https://cert-manager.io/docs/configuration/selfsigned/)
### [Ali DNS](https://github.com/pragkent/alidns-webhook)
### [Godaddy DNS](https://github.com/snowdrop/godaddy-webhook)

**注意:** 修改 **k8s/alidns/alidns-cluster-issuer.yaml** 内的 **YOUR-LETSENCRYPT-EMAIL** 设置成正确的邮箱

---

## Jenkins

Jenkins UI 涉及的环境变量和可选值

| 环境变量          | 值          | 说明                                                         |
|:------------------|:------------|:-------------------------------------------------------------|
| BRANCH_NAME       |             |                                                              |
| DEPLOY_ENV        | dev/prod    | dev: 开发环境使用自签名证书, prod: 使用 letsencrypt 获取证书 |
| DNS_VENDOR        | ali/godaddy |                                                              |
| DNS_VENDOR_BRANCH | master      | 依赖的 dns-vender 仓库分支                                   |

---

## dns vender

由于此仓库需要访问第三方的 DNS 服务，属于隐私信息所以需要配合私有仓库 **dns-vender** 主要记录了第三方的 DNS 服务的一些私钥信息，用于创建 **secret** 资源

文件夹名字和内部的文件命名规范

```
{vender}dns-secret
  - access-key
  - secret-key
```

## 提供的签名机构名称(申请证书需要填写的 ClusterIssuer name)

| ClusterIssuer | Name                      |
|:--------------|:--------------------------|
| selfsign      | selfsigned-cluster-issuer |
| alidns        | alidns-cluster-issuer     |
| godaady       | godaadyns-cluster-issuer  |
