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

| 环境变量      | 值          | 说明                                                         |
|:--------------|:------------|:-------------------------------------------------------------|
| BRANCH_NAME   |             |                                                              |
| DEPLOY_TARGET | true        |                                                              |
| DNS_VENDOR    | ali/godaddy |                                                              |

---

## 整体的操作流程

1. 注册域名, 并解析
2. 在相应的域名申请平台,申请 API 访问的 key 并记录到私有仓库 dns-vendor 仓库中(有相应的存放规范)
3. 部署 cert-manager 需要按照 **k8s/alidns** 编写
4. 部署 domain-certificate 需要按照 **k8s/ali-tls**
5. 完成上述步骤, 证书就申请完成了

## [可能的错误处理](./delete-ns.md)
