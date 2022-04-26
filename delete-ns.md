# 错误处理

如果在删除 **cert-manager** 的过程中出现错误，发现时命名空间无法删除，可按照以下方式处理

1. 查询当前命名空间的信息

```
  kubectl get ns cert-manager -o json > tmp.json
```

输出的信息类似(部分信息)

```json
"spec": {
  "finalizers": [
    "kubernetes"
  ]
}
```

2. 编辑 **tmp.json**，删除 **spec** 内的信息
```json
"spec": {

}
```

3. 另一起一个进程
```
nohup kubectl proxy &
```

4. 执行以下命令

```
curl -k -H "Content-Type: application/json" -X PUT --data-binary @tmp.json https://127.0.0.1:8001/api/v1/namespaces/<your-namespace>/finalize
```

5. 停止 **3** 开启的进程