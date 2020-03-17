# ar_os_scc_binding

Enable setting of Security Context Constraints against an Openshift
Service Account

## Requirements
- oc command line is available
- Execution is made by user able to add users to groups (oc adm policy add-scc-to-user) 


## Role Variables
The following details:
- the parameters that should be passed to the role (aka vars)
- the defaults that are held
- the secrets that should generally be sourced from an ansible vault.

### Parameters:
| Variable                    | Description                                 | Default |
| --------                    | -----------                                 | ------- |
| ar_os_scc_binding_namespace | Namespace on which to operate               | None    |
| ar_os_scc_binding_items     | Object defining the SCC Binding (See below) | []      |

The structure of the 'ar_os_scc_binding_items' is:
```
  {
    sa_name: "<the service account name>",
    scc: "<name of the scc to ascribe to the service account>",
  }
```
### Defaults
| Variable                     | Description                              | Default                                                                                                                        |
| --------                     | -----------                              | -------                                                                                                                        |
| ar_os_scc_binding_assertions | List of assertions made before execution | 'ar_os_scc_binding_namespace' is provided and for each item in 'ar_os_scc_binding_items' both 'scc' and 'sa_name' sre provided |

### Secrets
The following variables should be provided through an encrypted source:

## Example Playbook

```
- hosts: localhost
  tasks:
    - name: Include SCC Binding
      include_role:
        name: ar_os_scc_binding
      vars:
        ar_os_scc_binding_namespace: "sunshine-desserts"
        ar_os_scc_binding_items: [{sa_name: 'hypo-sa', scc: 'anyuid'}]
```