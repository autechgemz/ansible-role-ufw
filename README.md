# Ansible Role: ufw

## Description

Manage Uncomplicated Firewall(UFW) service.

## Requirements

None

## Dependencies

None

## OS Platforms

- Ubuntu focal+

## Example Playbook

```yaml
- hosts: all
  roles:
    - ufw
```

## Role Variables

### ufw_package_ensure: (string)

```yaml
ufw_package_ensure: 'present'
```

### ufw_debug: (bool)

```yaml
ufw_debug: false
```

### ufw_service_ensure: (string)

```yaml
ufw_service_ensure: 'started'
```

### ufw_service_enable: (bool)

```yaml
ufw_service_enable: true
```

### ufw_status: (bool)

```yaml
ufw_status: true
```

### ufw_logging_level: (off, low, medium, high, full)

```yaml
ufw_logging_level: 'low'
```

### ufw_config_manage: (bool)

```yaml
ufw_config_manage: true
```

### ufw_config_options: (dict)

```yaml
ufw_config_options:
  IPV6: "no"
  DEFAULT_INPUT_POLICY: "ACCEPT"
  DEFAULT_OUTPUT_POLICY: "ACCEPT"
  DEFAULT_FORWARD_POLICY: "DROP"
  DEFAULT_APPLICATION_POLICY: "SKIP"
  MANAGE_BUILTINS: 'no'
  IPT_SYSCTL: "/etc/ufw/sysctl.conf"
  IPT_MODULES: ""
```

### ufw_sysctl_manage: (bool)

```yaml
ufw_sysctl_manage: true
```

### ufw_config_sysctl_options: (dict)

```yaml
ufw_config_sysctl_options:
  net/ipv4/conf/all/accept_redirects: 0
  net/ipv4/conf/default/accept_redirects: 0
  net/ipv6/conf/all/accept_redirects: 0
  net/ipv6/conf/default/accept_redirects: 0
  net/ipv4/icmp_echo_ignore_broadcasts: 1
  net/ipv4/icmp_ignore_bogus_error_responses: 1
  net/ipv4/icmp_echo_ignore_all: 0
  net/ipv4/conf/all/log_martians: 0
  net/ipv4/conf/default/log_martians: 0
```

### ufw_policy_empty_purge: (bool)

```yaml
ufw_policy_empty_purge: false
```

### ufw_policy_manage: (bool)

```yaml
ufw_policy_manage: false
```

### ufw_policy_data: (dict)

```yaml
ufw_policy_data: {}
 - ensure: 'present'    # ['present','absent']
   policy: 'allow'      # ['allow','deny','reject']
   proto: 'tcp'         # ['tcp','udp','any']
   direction: 'in'      # ['in', 'out']
   interface: ''        # interface_name
   from: 'any'          # or any
   to: 'any'            # or any
   port: '22'           # or any
```

### ufw_after_rules_manage: (bool)

```yaml
ufw_after_rules_manage: false
```

### ufw_after_rules_data: (list)

```yaml
ufw_after_rules_data:
  - '*filter'
  - ':ufw-after-input - [0:0]'
  - ':ufw-after-output - [0:0]'
  - ':ufw-after-forward - [0:0]'
  - '-A ufw-after-input -p udp --dport 137 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -p udp --dport 138 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -p tcp --dport 139 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -p tcp --dport 445 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -p udp --dport 67 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -p udp --dport 68 -j ufw-skip-to-policy-input'
  - '-A ufw-after-input -m addrtype --dst-type BROADCAST -j ufw-skip-to-policy-input'
  - 'COMMIT'
```

### ufw_after6_rules_manage: (bool)

```yaml
ufw_after6_rules_manage: false
```

### ufw_after6_rules_data: (list)

```yaml
ufw_after6_rules_data:
- '*filter'
- ':ufw6-after-input - [0:0]'
- ':ufw6-after-output - [0:0]'
- ':ufw6-after-forward - [0:0]'
- '-A ufw6-after-input -p udp --dport 137 -j ufw6-skip-to-policy-input'
- '-A ufw6-after-input -p udp --dport 138 -j ufw6-skip-to-policy-input'
- '-A ufw6-after-input -p tcp --dport 139 -j ufw6-skip-to-policy-input'
- '-A ufw6-after-input -p tcp --dport 445 -j ufw6-skip-to-policy-input'
- '-A ufw6-after-input -p udp --dport 546 -j ufw6-skip-to-policy-input'
- '-A ufw6-after-input -p udp --dport 547 -j ufw6-skip-to-policy-input'
- 'COMMIT'
```

### ufw_before_rules_manage: (bool)

```yaml
ufw_before_rules_manage: false
```

### ufw_before_rules_data: (list)

```yaml
ufw_before_rules_data:
  - '*filter'
  - ':ufw-before-input - [0:0]'
  - ':ufw-before-output - [0:0]'
  - ':ufw-before-forward - [0:0]'
  - ':ufw-not-local - [0:0]'
  - '-A ufw-before-input -i lo -j ACCEPT'
  - '-A ufw-before-output -o lo -j ACCEPT'
  - '-A ufw-before-input -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw-before-output -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw-before-forward -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw-before-input -m conntrack --ctstate INVALID -j ufw-logging-deny'
  - '-A ufw-before-input -m conntrack --ctstate INVALID -j DROP'
  - '-A ufw-before-input -p icmp --icmp-type destination-unreachable -j ACCEPT'
  - '-A ufw-before-input -p icmp --icmp-type time-exceeded -j ACCEPT'
  - '-A ufw-before-input -p icmp --icmp-type parameter-problem -j ACCEPT'
  - '-A ufw-before-input -p icmp --icmp-type echo-request -j ACCEPT'
  - '-A ufw-before-forward -p icmp --icmp-type destination-unreachable -j ACCEPT'
  - '-A ufw-before-forward -p icmp --icmp-type time-exceeded -j ACCEPT'
  - '-A ufw-before-forward -p icmp --icmp-type parameter-problem -j ACCEPT'
  - '-A ufw-before-forward -p icmp --icmp-type echo-request -j ACCEPT'
  - '-A ufw-before-input -p udp --sport 67 --dport 68 -j ACCEPT'
  - '-A ufw-before-input -j ufw-not-local'
  - '-A ufw-not-local -m addrtype --dst-type LOCAL -j RETURN'
  - '-A ufw-not-local -m addrtype --dst-type MULTICAST -j RETURN'
  - '-A ufw-not-local -m addrtype --dst-type BROADCAST -j RETURN'
  - '-A ufw-not-local -m limit --limit 3/min --limit-burst 10 -j ufw-logging-deny'
  - '-A ufw-not-local -j DROP'
  - '-A ufw-before-input -p udp -d 224.0.0.251 --dport 5353 -j ACCEPT'
  - '-A ufw-before-input -p udp -d 239.255.255.250 --dport 1900 -j ACCEPT'
  - 'COMMIT'
```

### ufw_before6_rules_manage: (bool)

```yaml
ufw_before6_rules_manage: false
```

### ufw_before6_rules_data: (list)

```yaml
ufw_before6_rules_data:
  - '*filter'
  - ':ufw6-before-input - [0:0]'
  - ':ufw6-before-output - [0:0]'
  - ':ufw6-before-forward - [0:0]'
  - '-A ufw6-before-input -i lo -j ACCEPT'
  - '-A ufw6-before-output -o lo -j ACCEPT'
  - '-A ufw6-before-input -m rt --rt-type 0 -j DROP'
  - '-A ufw6-before-forward -m rt --rt-type 0 -j DROP'
  - '-A ufw6-before-output -m rt --rt-type 0 -j DROP'
  - '-A ufw6-before-input -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw6-before-output -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw6-before-forward -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type echo-reply -j ACCEPT'
  - '-A ufw6-before-input -m conntrack --ctstate INVALID -j ufw6-logging-deny'
  - '-A ufw6-before-input -m conntrack --ctstate INVALID -j DROP'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type destination-unreachable -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type packet-too-big -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type time-exceeded -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type parameter-problem -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type echo-request -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type router-solicitation -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type router-advertisement -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type neighbor-solicitation -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type neighbor-advertisement -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 141 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 142 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 130 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 131 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 132 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 143 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 148 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 149 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 151 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 152 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 153 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type destination-unreachable -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type packet-too-big -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type time-exceeded -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type parameter-problem -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type echo-request -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type echo-reply -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type router-solicitation -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type neighbor-advertisement -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type neighbor-solicitation -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type router-advertisement -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 141 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 142 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 130 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 131 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 132 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 143 -s fe80::/10 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 148 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 149 -m hl --hl-eq 255 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 151 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 152 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-output -p icmpv6 --icmpv6-type 153 -s fe80::/10 -m hl --hl-eq 1 -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type destination-unreachable -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type packet-too-big -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type time-exceeded -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type parameter-problem -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type echo-request -j ACCEPT'
  - '-A ufw6-before-forward -p icmpv6 --icmpv6-type echo-reply -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 144 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 145 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 146 -j ACCEPT'
  - '-A ufw6-before-input -p icmpv6 --icmpv6-type 147 -j ACCEPT'
  - '-A ufw6-before-input -p udp -s fe80::/10 --sport 547 -d fe80::/10 --dport 546 -j ACCEPT'
  - '-A ufw6-before-input -p udp -d ff02::fb --dport 5353 -j ACCEPT'
  - '-A ufw6-before-input -p udp -d ff02::f --dport 1900 -j ACCEPT'
  - 'COMMIT'
```

## Example vars

```yaml
ufw_policy_manage: true
ufw_policy_data:
  - ensure: 'present'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '22'
  - ensure: 'absent'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '636'
  - ensure: 'absent'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '3128'
  - ensure: 'absent'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '8080'
  - ensure: 'absent'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '443'
  - ensure: 'absent'
    policy: 'allow'
    proto: 'tcp'
    direction: 'in'
    interface: ''
    from: 'any'
    to: 'any'
    port: '8443'
```