---
sidebar_label: Keystone
---

# Keystone

## Domain manager role

| SCS Standard Track | SCS Standard | SCS Documentation |
|--------------------|--------------|-------------------|
| [IAM](https://docs.scs.community/standards/iam/) | [scs-0302](https://github.com/SovereignCloudStack/standards/blob/main/Standards/scs-0302-v1-domain-manager-role.md) | [Domain Manager configuration for Keystone](https://docs.scs.community/standards/scs-0302-v1-domain-manager-role/) |

To configure and use the domain manager role from the SCS project, the
`environments/kolla/files/overlays/keystone/policy.yaml` file is created
in the configuration repository. The deployment and upgrade of the Keystone
service itself is then done as usual.

```yaml title="environments/kolla/files/overlays/keystone/policy.yaml"
---
# SCS Domain Manager policy configuration

# Section A: OpenStack base definitons
# The entries beginning with "base_<rule>" should be exact copies of the
# default "identity:<rule>" definitions for the target OpenStack release.
# They will be extended upon for the domain manager role below this section.
"base_get_domain": "(role:reader and system_scope:all) or token.domain.id:%(target.domain.id)s or token.project.domain.id:%(target.domain.id)s"
"base_list_domains": "(role:reader and system_scope:all)"
"base_list_roles": "(role:reader and system_scope:all)"
"base_get_role": "(role:reader and system_scope:all)"
"base_list_users": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.domain_id)s)"
"base_get_user": "(role:reader and system_scope:all) or (role:reader and token.domain.id:%(target.user.domain_id)s) or user_id:%(target.user.id)s"
"base_create_user": "(role:admin and system_scope:all) or (role:admin and token.domain.id:%(target.user.domain_id)s)"
"base_update_user": "(role:admin and system_scope:all) or (role:admin and token.domain.id:%(target.user.domain_id)s)"
"base_delete_user": "(role:admin and system_scope:all) or (role:admin and token.domain.id:%(target.user.domain_id)s)"
"base_list_projects": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.domain_id)s)"
"base_get_project": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.project.domain_id)s) or project_id:%(target.project.id)s"
"base_create_project": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.project.domain_id)s)"
"base_update_project": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.project.domain_id)s)"
"base_delete_project": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.project.domain_id)s)"
"base_list_user_projects": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.user.domain_id)s) or user_id:%(target.user.id)s"
"base_check_grant": "(role:reader and system_scope:all) or ((role:reader and domain_id:%(target.user.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:reader and domain_id:%(target.user.domain_id)s and domain_id:%(target.domain.id)s) or (role:reader and domain_id:%(target.group.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:reader and domain_id:%(target.group.domain_id)s and domain_id:%(target.domain.id)s)) and (domain_id:%(target.role.domain_id)s or None:%(target.role.domain_id)s)"
"base_list_grants": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.user.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:reader and domain_id:%(target.user.domain_id)s and domain_id:%(target.domain.id)s) or (role:reader and domain_id:%(target.group.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:reader and domain_id:%(target.group.domain_id)s and domain_id:%(target.domain.id)s)"
"base_create_grant": "(role:admin and system_scope:all) or ((role:admin and domain_id:%(target.user.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:admin and domain_id:%(target.user.domain_id)s and domain_id:%(target.domain.id)s) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.domain.id)s)) and (domain_id:%(target.role.domain_id)s or None:%(target.role.domain_id)s)"
"base_revoke_grant": "(role:admin and system_scope:all) or ((role:admin and domain_id:%(target.user.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:admin and domain_id:%(target.user.domain_id)s and domain_id:%(target.domain.id)s) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.project.domain_id)s) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.domain.id)s)) and (domain_id:%(target.role.domain_id)s or None:%(target.role.domain_id)s)"
"base_list_role_assignments": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.domain_id)s)"
"base_list_groups": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.group.domain_id)s)"
"base_get_group": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.group.domain_id)s)"
"base_create_group": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.group.domain_id)s)"
"base_update_group": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.group.domain_id)s)"
"base_delete_group": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.group.domain_id)s)"
"base_list_groups_for_user": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.user.domain_id)s) or user_id:%(user_id)s"
"base_list_users_in_group": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.group.domain_id)s)"
"base_remove_user_from_group": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.user.domain_id)s)"
"base_check_user_in_group": "(role:reader and system_scope:all) or (role:reader and domain_id:%(target.group.domain_id)s and domain_id:%(target.user.domain_id)s)"
"base_add_user_to_group": "(role:admin and system_scope:all) or (role:admin and domain_id:%(target.group.domain_id)s and domain_id:%(target.user.domain_id)s)"

# Section B: Domain Manager Extensions

# classify domain managers with a special role
"is_domain_manager": "role:domain-manager"

# specify a rule that whitelists roles which domain admins are permitted
# to assign and revoke within their domain
"is_domain_managed_role": "'member':%(target.role.name)s or 'load-balancer_member':%(target.role.name)s or 'creator':%(target.role.name)s"

# allow domain admins to retrieve their own domain (does not need changes)
"identity:get_domain": "rule:base_get_domain or rule:admin_required"

# list_domains is needed for GET /v3/domains?name=... requests
# this is mandatory for things like
# `create user --domain $DOMAIN_NAME $USER_NAME` to correctly discover
# domains by name
"identity:list_domains": "rule:is_domain_manager or rule:base_list_domains or rule:admin_required"

# list_roles is needed for GET /v3/roles?name=... requests
# this is mandatory for things like `role add ... $ROLE_NAME`` to correctly
# discover roles by name
"identity:list_roles": "rule:is_domain_manager or rule:base_list_roles or rule:admin_required"

# get_role is needed for GET /v3/roles/{role_id} requests
# this is mandatory for the OpenStack SDK to properly process role assignments
# which are issued by role id instead of name
"identity:get_role": "(rule:is_domain_manager and rule:is_domain_managed_role) or rule:base_get_role or rule:admin_required"

# allow domain admins to manage users within their domain
"identity:list_users": "(rule:is_domain_manager and token.domain.id:%(target.domain_id)s) or rule:base_list_users or rule:admin_required"
"identity:get_user": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_get_user or rule:admin_required"
"identity:create_user": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_create_user or rule:admin_required"
"identity:update_user": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_update_user or rule:admin_required"
"identity:delete_user": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_delete_user or rule:admin_required"

# allow domain admins to manage projects within their domain
"identity:list_projects": "(rule:is_domain_manager and token.domain.id:%(target.domain_id)s) or rule:base_list_projects or rule:admin_required"
"identity:get_project": "(rule:is_domain_manager and token.domain.id:%(target.project.domain_id)s) or rule:base_get_project or rule:admin_required"
"identity:create_project": "(rule:is_domain_manager and token.domain.id:%(target.project.domain_id)s) or rule:base_create_project or rule:admin_required"
"identity:update_project": "(rule:is_domain_manager and token.domain.id:%(target.project.domain_id)s) or rule:base_update_project or rule:admin_required"
"identity:delete_project": "(rule:is_domain_manager and token.domain.id:%(target.project.domain_id)s) or rule:base_delete_project or rule:admin_required"
"identity:list_user_projects": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_list_user_projects or rule:admin_required"

# allow domain managers to manage role assignments within their domain
# (restricted to specific roles by the 'is_domain_managed_role' rule)
#
# project-level role assignment to user within domain
"is_domain_user_project_grant": "token.domain.id:%(target.user.domain_id)s and token.domain.id:%(target.project.domain_id)s"
# project-level role assignment to group within domain
"is_domain_group_project_grant": "token.domain.id:%(target.group.domain_id)s and token.domain.id:%(target.project.domain_id)s"
# domain-level role assignment to group
"is_domain_level_group_grant": "token.domain.id:%(target.group.domain_id)s and token.domain.id:%(target.domain.id)s"
# domain-level role assignment to user
"is_domain_level_user_grant": "token.domain.id:%(target.user.domain_id)s and token.domain.id:%(target.domain.id)s"
"domain_manager_grant": "rule:is_domain_manager and (rule:is_domain_user_project_grant or rule:is_domain_group_project_grant or rule:is_domain_level_group_grant or rule:is_domain_level_user_grant)"
"identity:check_grant": "rule:domain_manager_grant or rule:base_check_grant or rule:admin_required"
"identity:list_grants": "rule:domain_manager_grant or rule:base_list_grants or rule:admin_required"
"identity:create_grant": "(rule:domain_manager_grant and rule:is_domain_managed_role) or rule:base_create_grant or rule:admin_required"
"identity:revoke_grant": "(rule:domain_manager_grant and rule:is_domain_managed_role) or rule:base_revoke_grant or rule:admin_required"
"identity:list_role_assignments": "(rule:is_domain_manager and token.domain.id:%(target.domain_id)s) or rule:base_list_role_assignments or rule:admin_required"


# allow domain managers to manage groups within their domain
"identity:list_groups": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or (role:reader and system_scope:all) or rule:base_list_groups or rule:admin_required"
"identity:get_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or (role:reader and system_scope:all) or rule:base_get_group or rule:admin_required"
"identity:create_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or rule:base_create_group or rule:admin_required"
"identity:update_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or rule:base_update_group or rule:admin_required"
"identity:delete_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or rule:base_delete_group or rule:admin_required"
"identity:list_groups_for_user": "(rule:is_domain_manager and token.domain.id:%(target.user.domain_id)s) or rule:base_list_groups_for_user or rule:admin_required"
"identity:list_users_in_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s) or rule:base_list_users_in_group or rule:admin_required"
"identity:remove_user_from_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s and token.domain.id:%(target.user.domain_id)s) or rule:base_remove_user_from_group or rule:admin_required"
"identity:check_user_in_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s and token.domain.id:%(target.user.domain_id)s) or rule:base_check_user_in_group or rule:admin_required"
"identity:add_user_to_group": "(rule:is_domain_manager and token.domain.id:%(target.group.domain_id)s and token.domain.id:%(target.user.domain_id)s) or rule:base_add_user_to_group or rule:admin_required"
```

The role `domain-manager` is created using the OpenStack CLI. Alternatively, the role can
be added using Ansible or other tools.

```
$ openstack --os-cloud admin \
    role create \
    --or-show \
    --description "Domain Manager Role" \
    domain-manager
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | Domain Manager Role              |
| domain_id   | None                             |
| id          | 9b7140bfe628468ab9b86b365f9ac4c2 |
| name        | domain-manager                   |
| options     | {}                               |
+-------------+----------------------------------+
```

A user can then be made a domain manager for a particular domain by assigning this role.

```
$ openstack --os-cloud admin \
    role add \
    --user test \
    --domain test \
    domain-manager
```

## OIDC Federation
