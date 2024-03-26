## Application Service > ROLE > Release note

### March 26, 2024
#### Added Features
* [RESTful API] Changed the Get a list of users API.
    * POST /role/v3.0/appkeys/{appKey}/users/search : Get a list of users
        * For more information, see: [link](https://docs.nhncloud.com/en/Application%20Service/ROLE/en/api-v3-guide/#get-a-list-of-users )

### January 23, 2024.
#### Added Features
* Added attribute-based access control (ABAC) feature. 
  * [Console] Applied a new design and added condition attribute feature.
      * You can add the condition attribute feature to a role.
      * Added the 'Condition attribute' tab.
      * Added the ‘Manage’ tab.
  * [RESTFUL API] Public API v3 has been launched.
      * Provides role-based access control (RBAC) and attribute-based access control (ABAC) for roles.
      * Allows for diverse and fine-grained user access control by enabling specific roles or conditions.
  * [SDK] 2.0.0 was released.
      * Applied to v3 newly provided by RESTFUL API.

#### End of Support
* [Console] Excel upload/Excel download feature is not supported.
* [RESTFUL API] Public API v1 does not support the validation period setting feature when giving Role to User.
	* This feature is provided by attribute-based access control (ABAC).
* [SDK] The 1.x version does not support the validation period setting feature when giving Role to User.
	* This feature is provided by attribute-based access control (ABAC).

### September 26, 2023
#### Feature Updates
* Role ID naming rules changed when creating a role.
    * The maximum number of characters in RoleID has increased from 32 to 128.
    * Special characters `.` and `:` have also been added to allow, and previously only allowed `_` and `-`.

### November 26, 2019
#### Added Features
* When granting Role to User, you can set the validation period.
    * Role granted to the User is valid only within the validation period that you set; conversely, the privileges granted are always valid unless set. 
    * The validation period can be set in 'day' unit. 
    * Start date of the validation period can be set beyond the current point, and the permissions granted are valid from the date you set.
    * The validation period can only be set to the start date, in which case the granted permissions are always valid from the set date.
* You can set the order of exposure, tag in the role.
    * When searching for a role list, it sorts in the order of exposure.
    * Multiple tags are configurable and can be used as search keywords. 

### June 25, 2019
#### Added Features
* [Console] You can set the Trailing Slash setting for Resource Path.
    * When setting up Non-Identical Path, "/admin" and "/admin/" are recognized as different paths.
    * When setting up Identical Path, "/admin" and "/admin/" are recognized as the same path.

### February 22, 2018
#### Added Features
* [Console] Among the resource entries, path supports antiPathPattern. 
    * When setting "/admin/\*\*", you can support authorization checks with the resource path under admin.
* [Console] For ease of management, RoleName and RoleGroup have been added among Role items.
    * RoleName : You can give the Role a meaningful name to manage it.
    * RoleGroup : You can specify a group and manage it through a group-by-group search.
* [Console] Resource ID of the resource has been increased to 64 characters in length.
* [RESTFUL API] Among the Role entries, RoleName and RoleGroup have been added to extend the Role-related API.
    * For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)
* [SDK] Released as 1.1.7 .
    * Commons-collection 3.2.2 was applied to enhance security.
    

### 1.0.1
#### Added Features
* [RESTFUL API] Added the API to look up the list of each component.
	* GET /role/v1.0/appkeys/{appKey}/roles : role list look up
		* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)
	* GET /role/v1.0/appkeys/{appKey}/resources : resource list lookup
		* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#4-resource)
	* GET /role/v1.0/appkeys/{appKey}/scopes : scope list lookup
		* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#2-scope)
	* GET /role/v1.0/appkeys/{appKey}/operations : operation list lookup
		* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#5-operation)

#### Feature Updates
* [Console] You can enter Korean in Resource name. All characters can be entered except '/' characters.
* [Console] When entering and correcting Resource, Role, User, Scope, the message that shows when field validation fails has been modified.
	* The resource priority, description, has been improved to show phrases other than 4XX and 5XX errors when validating them.
		* Priority validation failure statement: 'Priority can only be entered with numbers (-32768 to 32767)'
		* description validation failed phrase: 'This is an invalid type of description.'
	* If the description of Scope, Role, and User is more than 128 characters, it has been improved to show phrases other than 4XX and 5XX errors.
		* description validation failed phrase: 'This is an invalid type of description.'
	* If you enter the Role or Scope that is not in the User fix screen, it has been improved to show the phrase other than the 11001 or 13001 error.
		* When entering a missing Scope, phrase: 'Scope ID not found'
		* When entering a missing Role, phrase: 'Role ID not found'
* [Console] Added the auto complete feature to the Operation field in Resource search screen.
* [Console] To prevent misuse of the Migration feature, a cautionary note has been added to the screen.
	* a cautionary note: '※ caution : Copy the current project's Resources, Role, Operation to the selected project. Delete existing Resources, Role, Operation for the selected project.'
* [RESTFUL API] API constraints has changed.
	* GET /role/v1.0/appkeys/{appKey}/resources/hierarchy The API has been changed to give full results without having to give users or roles as factors..
		* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#4-resource)

#### Bug Fixes
* [Console] On Resource Modification screen, an error that causes a 5XX error when changing the name of a resource with a sub-resource has been fixed. 
	* The normally changed UiPath is also reflected in the child resource. 
* [Console]  Fixed an error in the Resource search screen that caused all resources to be searched when searching for a user that does not exist. 
	* There are no resources shown in Resource Tree.
* [Console] Fixed an error when modifying a resource with a duplicate name that appeared to be the value applied when canceling after a modification request. 
	* It appears to be in the previous state.
* [Console]  Fixed an error in which the Total value was not changed properly when searching with the keyword description on the Role search screen.
	* The total value is reflected as the number of retrieved values.
* [Console] Fixed an error in which adding or deleting a role while being searched on the user screen was not reflected in the immediately searched list screen.
	* It is immediately reflected in the User List screen.
* [Console] On User Modification screen, the title has been modified to appear from 'Add User' to 'Modify User'.
 

### July 20, 2017
#### Bug Fixes  
* [Console] Failure warning window is displayed on the screen without reflecting when registering/modifying to the names of Resource, Role, and Scope that are already in use.  
	
### May 25, 2017
#### Bug Fixes
* [Console] Fixed an issue in which [ Excel Upload] feature on the Resource tab does not work 

### April 20, 2017
#### Bug Fixes
* Fixed a bug that returns error if value of userId(key) contains '.', '@'

### December 22, 2016
#### Feature Updates
* Added bulk user list lookup API
* Added the association lookup API linked to Scope

#### Bug Fixes
* Fixed an issue with role associations not functioning properly
* Fixed an issue where ScopeID ALL is not detected when searching for a user
* Fixed a bug that caused the hierarchy tree to return abnormally if there was an invalid resource tree

### 1.0.1
#### Feature Updates
* Added a feature to remove Cache from Client SDK and servers

#### Bug Fixes
* Modified Resource Path so that it cannot be modified to an invalid path that does not start with '/' when modifying it
	* Data input using Excel may not be possible if there is an incorrect path

### 1.0.1
#### Feature Updates
* Added the option to return users with associated Role when viewing user lists
	* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#1-user)

### 1.0.1
#### Feature Updates
* Added API to delete existing registered roles with the same scope when granting a new role to a user
* Added a User to a Role Add an option to create a User if it doesn't exist in the API
	* For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/api-guide/#3-role)

### 1.0.1
#### Feature Updates
* Polling API support is deprecated due to low usability
* Added feature to migrate data between projects using Role products
    * For more information, refer to the manual: [link](http://docs.nhncloud.com/ko/Application%20Service/ROLE/ko/console-guide/#_3)

#### Bug Fixes
* Fixed a bug that when you deleted a Role, but the association information for another Role was deleted
* Fixed a bug that causes intermittent permission checks to fail
