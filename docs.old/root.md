---
id: root
title: Root
sidebar_label: Root
---

The root is where the application's layout structure is defined. It is typically the first ui element the user interacts with. The root can be changed multiple times during the lifespan of the application. For example, if an app requires users to login to the app, it's common to use a stack based root and transition to either tabs or SideMenu based root if the login is successful.

## Setting root on app launch
RNN exposes an appLaunched listener which is invoked whenever root needs to be set.

On iOS, the app launched event is usually emitted once in the lifespan of the application - when the app is opened for the first time. On Android things become a little bit more complicated. Like on iOS, the event is emitted when the app is opened for the first time. Since the system can destroy the Activity when the app is in the background to free resources, the event is emitted when the app returns to foreground after the activity has been destroyed.

```js
Navigation.events().registerAppLaunchedListener(() => {
  Navigation.setRoot({
    root: {

    }
  });
});
```

> registerAppLaunchedListener() must be called as soon as the bundle is executed. Otherwise the event, which is emitted from native to JS, won't be handled when appropriate.

## Conditional initial root
A common use case is to set the initial root according to some condition. For example; if a user is logged in, show the applications main root, otherwise show a login screen. To do this, simply set the appropriate root according to your needs.
```js
Navigation.events().registerAppLaunchedListener(() => {
  if (isUserLoggedIn()) {
    setMainRoot();
  } else {
    setLoginRoot();
  }
});
```

## Common root structures
<!--DOCUSAURUS_CODE_TABS-->

<!--Stack Root -->
Stacks are usually used as root for small scale apps or for short lived flows within a big app, for example a login flow.
```js
Navigation.setRoot({
  root: {
    stack: {
      children: [
        {
          component: {
            name: 'LOGIN_SCREEN'
          }
        }
      ]
    }
  }
});
```

<!--BottomTabs Root-->
Typically, stacks are used as the direct children of BottomTabs and each child is an independent root. This lets users to seamlessly switch between tabs as each tab has its own navigation hierarchy. 
```js
Navigation.setRoot({
  root: {
    bottomTabs: {
      children: [
        stack: {
          children: [
            {
              component: {
                name: 'FEED_SCREEN'
              }
            }
          ]
        },
        stack: {
          children: [
            {
              component: {
                name: 'CHAT_LIST'
              }
            }
          ]
        },
        stack: {
          children: [
            {
              component: {
                name: 'PROFILE_SCREEN'
              }
            }
          ]
        }
      ]
    }
  }
});
```

<!--SideMenu Root-->
When a SideMenu layout is used as root, the center screen would typically be a stack. The center stack should be treated as a root - you can push child screens into it, or replace it all together by calling `setStackRoot`.

```js
Navigation.setRoot({
  root: {
    sideMenu: {
      center: {
        stack: {
          children: [
            name: 'HOME_SCREEN'
          ]
        }
      },
      left: {
        component: {
          name: 'MENU_SCREEN'
        }
      }
    }
  }
});
```
<!--END_DOCUSAURUS_CODE_TABS-->