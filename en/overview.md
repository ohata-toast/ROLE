## Application Service > ROLE > Overview

## What is Role service?

A service that allows you to systematically manage resource access control for users accessing your production environment.

## Overall Structure

![[Figure 1] Overall structure ](http://static.toastoven.net/prod_role/Role_Intro_01.png)
<center>[Figure 1] Overall structure </center>

### RBAC (Role-based access control)

You have to first define the required roles; roles can designate other roles as associated roles and can inherit other associated roles and condition attribute designated for those associated roles. This allows you to design a systematic role structure. 
You can define roles that can perform specific features and group these roles into associations with roles that can perform specific features.
You can assign roles to users that can perform a small range of features, or you can assign roles that can perform more features.

### ABAC (Attribute-based access control)

When designing authorization policy, roles alone may not be enough. You can configure detailed policy by defining roles based on condition attribute.
Condition attribute can be configured in `ID-Value` format and can be assigned to `user` or `role`. You can configure this to allow access if it matches or does not match the value given to the condition attribute.

ex) Define `bucket-name attribute ID` which defines the bucket name that each access destination has. It grants users the same condition attribute ID and acceptable property values.
For example, if `bucket-name attribute ID` is given an attribute value named `product` to the user, access is allowed only if the user has an attribute value matching `product` when accessing the target.

When you access a protected resource with a `specific attribute ID ` as in the example above, you can configure the resource to be allowed access only if the attribute value you give matches the condition attribute of the target you want to access.

### Resource

A resource is a unit that defines a protected resource. It can be configured as a URI-based hierarchy structure, and each resource can designate resource identification information with a list of `permissions (role-operation pairs)` to access resources.
It is useful when establishing a policy for authorization based on resources. 
However, if you establish a role-based access control policy, you do not need to define resources.

Example: You can define `posting` on the bulletin board as a resource so that `modify` and `delete` allow only `administrator role` and `guest role` allow only to `view`. Also, if you designate the role of `guest` to the role of `administrator` as the associated role to perform the `View` operation, enabling efficient management without duplicate application.

### User

The user's access rights are examined by the roles assigned to the user and the associated roles of those roles.
When you assign a role to a user, you can also specify the range of its validity, which is useful when you have multiple organizations or targets with the same role and condition attributes, and resource system in your operation environment.

Authorization is provided on a role-based and resource-based. The role-based checks to allow access to a role that you specify and also checks for attributes that are assigned to a user or role, if any.
Resource-based checks to allow access to user-specified resources and operations. Similarly, if a resource is given attributes in an accessible role, the attribute is also examined.

## Main Features

![[Figure 2] Feature Description](http://static.toastoven.net/prod_role/Role_Intro_02.png)
<center>[Figure 2] Feature Description</center>

* Provides ABAC for RBAC and roles.
* Allows for inheritance between roles, enabling you to organize a large number of roles.
* Allows for diverse and fine-grained user access control by enabling specific roles or conditions.
* Supports ant-path based REST API resources that contain a path variable.
* Provides role-based and resource-based user access control.
* Provides REST APIs and SDKs.

## Service Terms

| Term | Description                 |
| --- |--------------------|
| User | A subject who has a role         |
| Role    | Minimum unit for resource access control  |
| Range    | Validity range that a role can have    |
| operation | Action that a user can do to a resource |
| Resource   | All objects the role can access   |
| Condition attribute | Condition attribute that can be added to a role   |
