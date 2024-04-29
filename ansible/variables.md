---
title: .
title-slide-attributes:
  data-background-image: img/ansible.png
  data-background-size: contain
---

Ansible utilizes three main variable scopes

- Global Scope
- Play/Role Scope
- Host Scope

---

## Global Scope

This scope encompasses variables set outside the context of a specific playbook or role.

---

### Environment Variables

Variables set in your system environment.

```bash {.numberLines} 
    SOME_VAR="some-value"
```

> Ansible must read inside the playbook to use it

---

### Command Line

Variables passed during ansible commands

`-e` or `--extra-vars`

```bash {.numberLines}
ansible-playbook playbook.yml --extra-vars my_variable="my-value"
```

---

## Play Scope

This scope covers variables defined within a particular playbook or role.

---

### Vars section

Explicitly in the vars section of a playbook.

```yaml {.numberLines}
- name: Var in playbook
  hosts: all
  vars:
    my_var_from_play: foo
  tasks:

    - name: Var from play
      debug:
        var: my_var_from_play
```

---

### vars_files

Variables loaded from external YAML or JSON files in a playbook.

```yaml {.numberLines}
    - name: Var in playbook
      hosts: all
      vars_files: my-vars.yaml
```

*my-vars.yaml*
```yaml {.numberLines}
---
my_var: value
```

---

### vars_prompt

Variables obtained interactively during playbook execution

```yaml {.numberLines}
- name: Prompted var
  hosts: all
  vars_prompt:
    - name: username
      prompt: "username:"
      private: false
```

---

## Role scope

Role variables

---

### Role Defaults

Variables defined in the `defaults/main.yml` file of any included roles.

```yaml
# defaults/main.yml
---
overridable_variable: "default value"
```

---

### Role Variables

Variables defined in the `vars/main.yml` file of any included roles (overrides role defaults).

```yaml
# vars/main.yml
---
overriding_variable: "overrides roles variables"
```

---

## Host Scope

This scope pertains to variables associated with specific hosts or groups in your inventory.

---

### Inventory Variables

Variables defined directly for a host or group in your inventory file.

```yaml {.numberLines}   
# inventory.yaml
---
all:       
  vars:    
    datacenter: iad       # Global variable
  children:
    runners:
      hosts:
        runner1:
          name: runner1   # Host variable
        runner2:
          name: runner2   # Host variable
      vars:
        total_runners: 2  # Group variable
```

---

### include_vars

Variables loaded from external YAML or JSON files using the include_vars directive within a specific host or group.

```yaml {.numberLines}
- name: Load vars
  ansible.builtin.include_vars:
    file: my-vars.yaml
    name: varz
```

See more [examples here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_vars_module.html#examples)

---

### Host Facts

Information gathered about a host during playbook execution (automatically available as variables).

```yaml {.numberLines}
- name: Playbook
  hosts: all
  gather_facts: true               # Gathered by default
  tasks:
  
    - name: Distribution
      ansible.builtin.debug:
        var: ansible_distribution  # Gathered variable/fact
```

### Registered Variables

The output of tasks stored as variables for later use (using the `register` keyword).

```yaml {.nubmerLines}
- name: Get time
  ansible.builtin.shell:
    cmd: date --iso-8601=seconds
  register: time_now
```

---

### Block Variables

```yaml {.numberLines}
- name: Block
  when: conditional | default(false) | bool
  vars:
    answer: 42
  block:

    - name: Task A
      ansible.builtin.debug:
        var: answer

    - name: Task B
      ansible.builtin.debug:
        var: answer | int // 10

```

### Task Variables

```yaml {.numberLines}
- name: Task A
  vars:
    answer: 42
  ansible.builtin.debug:
    var: answer
```

## Variable Precedence

Ansible follows a specific order to determine which variable value takes precedence when multiple variables with the same name exist across different scopes. Variables with higher precedence override those with lower precedence. The order, from least to greatest precedence, is:

---

1. Command line values (not really a variable)
2. Role defaults
3. Inventory file or script (dynamic) **group** vars
4. Inventory `group_vars/all`
5. Playbook `group_vars/all`
6. Inventory `group_vars/*`
7. Playbook `group_vars/*`

---

8. Inventory file or script **host** vars
9. Inventory `host_vars/*`
10. Playbook `host_vars/*`
11. Host facts / cached `set_facts`
12. Play vars
13. Play `vars_prompt`
14. Play `vars_files`

---

15. Role vars (role/vars/main.yml)
16. Block vars (only for tasks in block)
17. Task vars (only for the task)
18. `include_vars`
19. `set_facts` / registered vars
20. `role` (and `include_role`) params
21. include params
22. Extra vars (`-e "var=value"`)

---

## Registered variables

- Stored in memory
- Can't be cached    
- Available in subsequent plays of the same run
                     
## References        
                     
- [Ansible Playbook variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)
- [Ansible Inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)
- [Ansible variables](https://docs.ansible.com/ansible/latest/reference_appendices/general_precedence.html#variables)


