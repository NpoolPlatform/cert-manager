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
| DEPLOY_TARGET | true        |                                                              |                                                        |

---

## 整体的操作流程

1. 注册域名, 并解析
2. 在相应的域名申请平台,申请 API 访问的 key 并记录到私有仓库 dns-vendor 仓库中(有相应的存放规范)
3. 部署 cert-manager 需要按照 **k8s/alidns** 编写
4. 部署 domain-certificate 需要按照 **k8s/ali-tls**
5. 完成上述步骤, 证书就申请完成了

## [Uninstall](https://cert-manager.io/docs/installation/kubectl/#uninstalling)

1. 前置查询

在删除 **cert-manager** 资源之前请确保以下资源都被清除干净**注意是全部命名空间下**

> kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces

如果确认以上资源都被清除，下面就可以删除 **cert-manager**, 不过可能导致 **已申请** 的证书被删除

2. 删除 **cert-manager** 资源

执行这一步骤之前确保

    1. 先删除 **cert-manager** 资源

    2. 然后再删除 **cert-manager** 所在的命名空间

---

## [可能的错误处理](./delete-ns.md)
