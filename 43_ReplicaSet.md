# Replica Set

Write a common use-case, where you will use a daemon set instead of replica set.

HW monitor can be such a use-case.

DaemonSet will create exactly one pod on each running node, which is usefull for monitoring for example the health of the nodes.
Whereas ReplicaSet will create defined number of pods on nodes based on their current resources consumption.
When a node is destroyed pods that were on that node are scheduled to be recreated on another living node.
- There is always exactly the number that is defined in configuration scheduled on available nodes

In case of DaemonSet, when a node is destroyed, pod that was living on that node is destroyed. When a node is added new pod is scheduled on that node.
- There is always exactly one pod scheduled on each node

e.g. ssd-monitor mentioned in the kubernetes-training/04-controllers/ssd-monitor-daemonset.yaml
