---
id: docTest
title: Row View
---

Display its children in row using ```flexDirection: 'row'```

```javascript
import React from 'react';
import { View, StyleSheet } from 'react-native';

export const RowContainer = ({ children, style = {}, ...props }) => (
    <View {...props} style={[styles.container, style]}>
        {children}
    </View>
);

const styles = StyleSheet.create({
    container: {
        flexDirection: 'row',
        justifyContent: 'center'
    }
});
```
