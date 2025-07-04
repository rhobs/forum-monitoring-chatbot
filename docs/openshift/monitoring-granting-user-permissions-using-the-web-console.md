:_mod-docs-content-type: PROCEDURE
[id="granting-user-permissions-using-the-web-console_{context}"]
# Granting user permissions by using the web console

You can grant users permissions for the `openshift-monitoring` project or their own projects, by using the OpenShift web console.

.Prerequisites

* You have access to the cluster as a user with the `cluster-admin` cluster role.
* The user account that you are assigning the role to already exists.

.Procedure

. In the *Administrator* perspective of the OpenShift web console, go to *User Management* -> *RoleBindings* -> *Create binding*.

. In the *Binding Type* section, select the *Namespace Role Binding* type.

. In the *Name* field, enter a name for the role binding.

. In the *Namespace* field, select the project where you want to grant the access.
+
# [IMPORTANT]
# The monitoring role or cluster role permissions that you grant to a user by using this procedure apply only to the project that you select in the *Namespace* field.

. Select a monitoring role or cluster role from the *Role Name* list.

. In the *Subject* section, select *User*.

. In the *Subject Name* field, enter the name of the user.

. Select *Create* to apply the role binding.
