# Kubernetes in Space

This is a proof-of-concept brainstorm of leveraging Kubernetes strenghts to deliver and host applications in Rockets, Ships, Space Stations and Sattelites.

## Why

The space is still full of uncertainty. Things break and go wrong in zero gravity. That includes Engines and **Software**. To work in these types of environment, the software and the underlying platform must be designed to fail, scale (up and down) as necessary and be as resource efficient as possible.

## What 

Kubernetes is an open source container orchestration platform that is completely extensible and works well in air-gapped environment (without internet connectivity). 

It is designed for fault tolerance and automatic scaling of either the pods (applications) and nodes (worker machines).

The resource usage of the required kubernetes components is extremely low for the benefits it brings to the table. With low overhead from the platform components, we can dedicate almost all of the resource available in the nodes for the applications.

## How

The ideia is to treat space as any other air-gapped environment (even if it's not air-gapped all the time).

- The underlying operational system needs to be as light and straightforward as possible so that the resources are dedicated primarily to the applications and OS problems can be avoided.
- The cluster are configured in a way that all necessary services and components boots up with the OS itself.
- [The control plane and etcd's are spread along the available workers](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/).
- [The applications](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) **and** [the nodes](https://github.com/kubernetes/autoscaler) have autoscaling features tweaked accordingly.
- [Monitoring for the cluster is configured](https://kubernetes.io/docs/concepts/cluster-administration/system-metrics/). [Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/oss/grafana/) are popular choices to use alongside the metric-server, for example.
- If using Prometheus, alerts can be set using [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) to warn mission control as well as other embedded systems.
- An operator can be leveraged to start and stop applications with few commands as possible and/or physical triggers.

## Benefits

- Unused nodes can be decoupled from the cluster without affecting overall function. We can embed a cluster node to one of the decoupling Fuel Stages from a Rocket and automatically remove it from the cluster after the decoupling. This means that we save weight and computer power from the rest of the rocket while it's attached. *We can look at nodes like lego parts that can be attached and detached as necessary*.
- Leverage redundancy to mitigate eventual node failures.
- We can monitor the system and platform metrics as well as the applications metrics. *The ideia is to monitor systems metrics the same way as Rocket Fuel, for example*.
- The rolling release deployment strategy (default to kubernetes) can be used for safer, no downtime deployments.
- Open source means it's extremely extensible and broadly available: Answers for common problems can be found online, which can be speed up development. We don't need to reinvent the wheel and can use the best tools the community has made available, which can lower the budget. The community can be asked for help, if necessary, and it's good practice to contribute back. [Here's a list of big companies that have contributed for the Kubernetes Ecosystem](https://k8s.devstats.cncf.io/d/9/companies-table?orgId=1). *Open source brings a lot to the table*.
- Multiple cluster can be monitored and maintained with the same "language". A new sattelite is being deployed into a fleet? From a SRE and SE point of view its just another cluster. Kubernetes adoption is already pretty high in multiple areas: engineers that have expertise would be easier to find and can get up to speed faster because *Kubernetes is kubernetes*.

## Impediments and Precautions

- The cluster (and applications) needs to be tested for network connectivity outbreaks. *The cluster needs to keep working in the dark*.
- Power outage needs to be accounted for. *Everything needs to set-up itself from scratch in case of a outage*.
- Find and mitigate **every** *single point of failure*.
