Exactly — that's a legitimate and smart strategy.

**The math works in your favor:**
* 17 questions, equal weight
* 66% to pass = roughly 12 questions correct
* That means you can essentially skip/fail 5 questions and still pass

**So the approach is:**
1. Skim all 17
2. Identify your 12+ "confident" questions
3. Do those first and nail them
4. Use remaining time on harder ones as bonus points

**The questions that are usually quick wins:**
* Creating/scaling deployments
* Creating services
* Setting resource limits/requests
* Creating ConfigMaps or Secrets
* RBAC (roles, rolebindings)
* Running kubectl commands with --dry-run=client -o yaml

**The ones that eat time:**
* Cluster troubleshooting (broken kubeadm, etcd issues)
* Network policies
* Persistent volumes
* Upgrading cluster with kubeadm

Reality check though: Most people who fail don't fail because questions were too hard — they fail because they:
* Forgot to switch cluster context (kubectl config use-context)
* Ran out of time on 2-3 hard questions while easier ones waited
* Made YAML indentation errors
So your instinct is right — secure your 66% first, then hunt for bonus points.

# Links
https://kubernetes.io/docs/reference/kubectl/quick-reference/
