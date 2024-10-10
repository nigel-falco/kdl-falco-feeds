# kdl-falco-feeds
Test Scripts for the Falco Feeds KDL

## Fixing falco upgrades

This did not work in the lab environment:

```
helm install falco falcosecurity/falco --namespace falco \
  --create-namespace \
  --set tty=true \
  --set falcosidekick.enabled=true \
  --set falcosidekick.webui.enabled=false \
  --set falcosidekick.webui.redis.storageEnabled=false \
  --set falcosidekick.config.webhook.address=http://falco-talon:2803 \
  --set "falcoctl.config.artifact.install.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
  --set "falcoctl.config.artifact.follow.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
  --set "falco.rules_file={/etc/falco/falco_rules.yaml,/etc/falco/falco-incubating_rules.yaml,/etc/falco/falco-sandbox_rules.yaml,/etc/falco/rules.d}" \
  -f custom-rules.yaml
```

The initial install that worked:
```
helm install falco falcosecurity/falco --namespace falco \
   --create-namespace \
   --set tty=true \
   --set driver.kind=modern_ebpf \
   --set falcosidekick.enabled=true \
   --set falcosidekick.webui.enabled=false \
   --set falcosidekick.webui.redis.storageEnabled=false \
   --set falcosidekick.config.webhook.address=http://falco-talon:2803 \
   --set collectors.containerd.enabled=true \
   --set collectors.containerd.socket=/run/k3s/containerd/containerd.sock
```

I also tried this ```helm upgrade```:
```
helm upgrade falco falcosecurity/falco --namespace falco \
   --create-namespace \
   --set tty=true \
   --set driver.kind=modern_ebpf \
   --set falcosidekick.enabled=false \
   --set falcosidekick.webui.enabled=false \
   --set falcosidekick.webui.redis.storageEnabled=false \
   --set falcosidekick.config.webhook.address=http://falco-talon:2803 \
   --set collectors.containerd.enabled=true \
   --set collectors.containerd.socket=/run/k3s/containerd/containerd.sock \
   --set "falcoctl.config.artifact.install.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
   --set "falcoctl.config.artifact.follow.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
   --set "falco.rules_file={/etc/falco/falco_rules.yaml,/etc/falco/falco-incubating_rules.yaml,/etc/falco/falco-sandbox_rules.yaml,/etc/falco/rules.d}"
```

Let's test the reuse of existing helm values:

```
helm upgrade falco falcosecurity/falco --namespace falco \
   --reuse-values \
   --set "falcoctl.config.artifact.install.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
   --set "falcoctl.config.artifact.follow.refs={falco-rules:2,falco-incubating-rules:2,falco-sandbox-rules:2}" \
   --set "falco.rules_file={/etc/falco/falco_rules.yaml,/etc/falco/falco-incubating_rules.yaml,/etc/falco/falco-sandbox_rules.yaml,/etc/falco/rules.d}"
```

