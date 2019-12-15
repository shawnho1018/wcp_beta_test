
Harbor enable失败的时候，无法disable。用以下方式删除，重新enable。


```
dcli +i +show-unreleased
dcli>com vmware vcenter content registries harbor list 
dcli>com vmware vcenter content registries harbor delete --registry <registry-id>
```
