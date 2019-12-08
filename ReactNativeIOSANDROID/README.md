Compare REACT Native versus iOS and Android application environment           

## Expo development tools        
1. Expo Client    
Run your projects before you deploy. Open projects by scanning QR codes.     

2. Expo CLI     
command line environment. Serve, share, build, and publish Expo projects.     

3. Expo Snack    
React Native in the browser. Snack lets you to run complete React Native projects in the browser. No download required.    

4. Expo SDK    
libraries. Access native device APIs in your Expo project.      


React Native is a framework created by Facebook that allows you to develop native mobile apps for iOS and Android with a single JavaScript codebase.    
JSX — a syntax that is used to embed XML with JavaScript.    

## Compare   
1. Operating system    
the recommended tool for React Native developers is a computer with macOS. In windows, we cannot download xcode, so we cannot develope ios.        
2. Native element    
while using elements from the React Native library, for example, using the Picker component produces a different result in the iOS simulator than in the Android emulator.     
3. Specific Styles - shadows    
Shadows are another important topic in terms of differences between iOS and Android while working on cross platform app.      
4. Linking Libraries    
sometimes the docs aren’t updated to the latest react-native version and there might be some differences or errors which may take some time to fix.    

## Demo app on IOS
Since my mac is too old, version of my xcode is not recent, I can only use React Native CLI Quickstart instead of complete Expo CLI.    
My target os for app is ios and my computer system is mac.    

### Environment set up    
Need Node, Watchman, the React Native command line interface, and Xcode.     
Follow the command line code on this: https://facebook.github.io/react-native/docs/getting-started     

### Demo app1 - "hello world"     
![demoImg1](https://github.com/zhou-1/State-Of-Art-Researches/blob/master/ReactNativeIOSANDROID/DemoImg/1.jpg =100x)

code is below:
```
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class HelloWorldApp extends Component {
  render() {
    return (
        <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
          <Text>Hello, world!</Text>
        </View>
    );
  }
}
```

### Demo app2 - pressed&Long-pressed button    
![demoImg2](https://github.com/zhou-1/State-Of-Art-Researches/blob/master/ReactNativeIOSANDROID/DemoImg/2.jpg)   
![demoImg3](https://github.com/zhou-1/State-Of-Art-Researches/blob/master/ReactNativeIOSANDROID/DemoImg/3.jpg)   
![demoImg4](https://github.com/zhou-1/State-Of-Art-Researches/blob/master/ReactNativeIOSANDROID/DemoImg/4.jpg)   

code is below:
```
import React, { Component } from 'react';
import { Platform, StyleSheet, Text, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback, TouchableWithoutFeedback, View } from 'react-native';

export default class Touchables extends Component {
  _onPressButton() {
    alert('Hello world from pressed button!')
  }

  _onLongPressButton() {
    alert('Hello world from long-pressed button!')
  }

  render() {
    return (
      <View style={styles.container}>
        <TouchableHighlight onPress={this._onPressButton} underlayColor="white">
          <View style={styles.button}>
            <Text style={styles.buttonText}>Hello world button</Text>
          </View>
        </TouchableHighlight>
        
        <TouchableOpacity onPress={this._onPressButton}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>Hello world opacity button</Text>
          </View>
        </TouchableOpacity>
        
        <TouchableHighlight onPress={this._onLongPressButton} onLongPress={this._onLongPressButton} underlayColor="white">
          <View style={styles.button}>
            <Text style={styles.buttonText}> Hello world from Long Press button</Text>
          </View>
        </TouchableHighlight>
      </View>
      
      
    );
  }
}

const styles = StyleSheet.create({
  container: {
    paddingTop: 60,
    alignItems: 'center'
  },
  button: {
    marginBottom: 30,
    width: 260,
    alignItems: 'center',
    backgroundColor: '#2196F3'
  },
  buttonText: {
    textAlign: 'center',
    padding: 20,
    color: 'white'
  }
});

```


## Run automated test using AWS test factory    


