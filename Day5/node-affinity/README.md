## List nodes with labels
```
oc get nodes --show-labels
```

## Label some nodes with our custom label
```
oc label node <your-node-name> disktype=ssd
oc label node master-1.ocp.tektutor.org distype=ssd
```

## Let's investigate the node label details now
```
oc get nodes --show-labels
```

## Let's schedule a pod on the node that has 
```
oc apply -f pod.yml 
```

List and see on which node that pod is deployed
```
oc get pods --output=wide
```
