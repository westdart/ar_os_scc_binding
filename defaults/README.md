scc_binding
===========

Enable setting of Security Context Constraints against an Openshift
Service Account

Requirements
------------


Role Variables
--------------

| Variable          | Description                                 | Default |
| --------          | -----------                                 | ------- |
| scc_binding_items | Object defining the SCC Binding (See below) | None    |


The structure of the 'scc_binding_items' is:
```
  {
    name: "<the service account name>",
    namespace: "<the namespace teh service account is in>",
    scc: "<name of the scc to ascibe to the service account>",
  }
```

Example Openshift Applier stanza
----------------

```
- object: Service Accounts for DEV
  content:
  - name: aspera-serviceaccount-serviceaccount
    namespace: "dif-dev"
    template: "serviceaccount-template.yml"
    action: apply
    params_from_vars:
      SA_NAME: aspera-sa
      SA_NAMESPACE: dif-dev
    tags:
    - projects
    - serviceaccounts
    - aspera
    - amqbroker
    - amqic
    post_steps:
      - role: scc_binding
        vars:
          tmp_dep_dir: "roles/"
          scc_binding_items: [{name: 'aspera-sa', namespace: 'dif-dev', scc: 'anyuid'}]

```