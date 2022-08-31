---
layout: post
title:  "GameBus Developer Portal"
date:   2021-03-25 10:06:51 +0200
---
Developer portal provides ''in-app functionalities for GameBus developers to efficiently create and test new (data provider) integrations without affecting challenges/circles unrelated to those development while keeping the admins in control of the permissions of specific developers.''

## Developers Portal Panel
This panel is only visible to users with a DEV role. Developer portal page can be found under the 'Developer' tab in the settings page - An admin panel page, only visible for users with an ADMIN role - Shows up in the navigation bar when a user has an ADMIN role.

Every user can request a DEV role by clicking the 'request' button under the 'Developer' tab in the settings page.

On the developers portal users can: - Create new DataProvider instances - Create new PropertyWritePermissions for DataProviders (or delete existing ones) - Request public access for certain PropertyWritePermissions associated with created DataProviders - Create beta Circles, used for testing DataProviders on existing users - Configure the callbackURL used to communicate when a user connects to a custom DataProvider.

On the admin panel users can: - Control DEV role requests of users - Control PropertyWritePermission requests of users.

![](/assets/images/developerPortal/DevPanel.PNG)

## Request Dev Role
Go to Settings>Developer and then click 'request developer role'.

You should then wait for the admin to approve/disapprove the respective request.

## Create New DataProvider
Go to DevPanel>Data Providers and then click the orange plus-sign on the top left. Fill in all necessary fields and then Click CREATE DATA PROVIDER.

![](/assets/images/developerPortal/DevPanel_CreateNewDataProvider.PNG)

The newly created provider can then be edited later if needed.

![](/assets/images/developerPortal/DevPanel_EditCreatedDataProvider_Type.PNG)

## Create A New Property Write Permission

Go to DevPanel>Data Providers and click on the DataProvider you want to create a property write permission for.
Click 'manage property permissions and then click on the orange plus sign.
Choose a gameDescriptor and property and click 'create property permission'.

![](/assets/images/developerPortal/DevPanel_ManagePropertyPermissions.PNG)

## Invite to betaCircle
Go to DevPanel>Data Providers and then click on the DataProvider you want to control the betaCircle for.
Click 'edit beta circle'. Click 'invite players'.
Search players and check boxes and then click 'invite players'.

![](/assets/images/developerPortal/DevPanel_UpdateCircle-InvitePlayers.PNG)

## Accept circle invite
You should accept the circle invite like any other circle invite by going to the 'Circles' tab.

## Test dataProvider by posting gameSessions on betaCircle
### Connect to DataProvider
Let beta player connect to dataProvider.
Go to Settings>Data. Click 'connect' for the respective DataProvider and go through the permission questions.

![](/assets/images/developerPortal/DevPanel_ConnectToDataProvider.PNG)

### Get player's oauth token via callbackUrl
GameBus will send a POST request to the callbackUrl of the respective DataProvider with the following request parameters:
* 'player_id': \<\<id of player connecting>>
* 'access_token': \<\<generated oauth token for player>>
* 'refresh_token': \<\<generated refresh token for player>>

### Post activities
Send POST request to `/activities` with the player's oauth token as a bearer token.
Make sure to only post on the gameDescriptors and properties you have PWPs (Property Write Permissions) for.
Make sure you include the right playerId (Id of the player you want to post for, i.e. the player associated with the bearer token).

### Request public access for PWPs
Go to DevPanel>Data Providers. Click on the DataProvider you want to control the betaCircle for.
Click 'manage property permissions'. Check the boxes for the PWPs you want to make public.
Click 'make public'.

![](/assets/images/developerPortal/DevPanel_ManagePropertyPermissions.PNG)

You should then wait for the admin to approve/disapprove the respective request.

