---
layout: post
title: "Using KeyboardAvoidingView with React Navigation"
---

When using React Navigation the status bar and header cause `KeyboardAvoidingView` to layout incorrectly.

To fix this you need to calculate the offset caused by the status bar and header and provide it to the `KeyboardAvoidingView` via its `keyboardVerticalOffset` prop.


## Getting the status bar height

### If you’re using Expo
You can get the status bar height from [Expo Constants](https://docs.expo.io/versions/latest/sdk/constants/).

`expo install expo-constants`

```
import Constants from 'expo-constants';

const statusBarHeight = Constants.statusBarHeight;
```


### If you’re not using Expo
The React Native API doesn’t yet provide a reliable cross-platform means of getting the status bar height. So we’ll use a package named [react-native-status-bar-height](https://www.npmjs.com/package/react-native-status-bar-height).

`yarn add react-native-status-bar-height` 

```
import { getStatusBarHeight } from 'react-native-status-bar-height';

const statusBarHeight = getStatusBarHeight();
```

## Getting the header height
To get the header height we use React Navigation’s `useHeaderHeight` hook.

```
import { useHeaderHeight } from 'react-navigation-stack';

const headerHeight = useHeaderHeight();
```


## Offsetting KeyboardAvoidingView
For convenience let’s create a wrapper component that calculates the vertical offset and passes it on to `KeyboardAvoidingView`. This snippet assumes you're using Expo.

```
import React from 'react';
import { KeyboardAvoidingView } from 'react-native';
import Constants from 'expo-constants';
import { useHeaderHeight } from 'react-navigation-stack';

export const KeyboardAwareView = ({ children }) => {
  const offset = useHeaderHeight() + Constants.statusBarHeight;

  return (
    <KeyboardAvoidingView
      behavior="padding"
      keyboardVerticalOffset={offset}
      style={{ flex: 1 }}
    >
      {children}
    </KeyboardAvoidingView>
  );
};
```
