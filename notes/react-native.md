---
title: React Native
tags:
  - react-native
emoji: 📔
link:
---

## Getting Started

```
// install
sudo npm install expo-cli --global

// start new project, can choose typescript template
expo init project-name

cd project-name

// install typescript types later
npm install --save-dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer

npm start
```

Styling is done via JS. CSS-like property names are offered by RN.

Install Android Studio to emulate development on the pc.

1. Download from: https://developer.android.com/studio#downloads
2. Follow installation instructions
3. Once installed, configure SDK Manager
4. Install appropriate SDK platforms
5. Install SDK Tools
	1. Android SDK Built-in Tools
	2. Android Emulator
	3. Android SDK Platform Tools
	4. Google Play Services
6. Configure AVD Manager
	5. Create Virtual Device
	6. Select a device (preferable with Google Play Services)
	7. Create the device
	8. Can start the device from AVD manager
7. To start expo in the emulator, run expo project and press 'a'
8. To reload the project, on Android press 'rr' (Cmd + r on iOS)

## Working With Core Components

<View> is like a div. It's used for layout and styling.

Text can only be put inside a <Text> component.

## Styling

Can either use inline styling or as a stylesheet object (preferred).

All elements have display flex by default, with flex-direction 'column'.

```jsx
// inline styling
<View>
	<View style={styles.screen}>
		<TextInput
			placeholder="Enter Goal"
			style={{
				borderBottomColor: "black",
				borderBottomWidth: 1,
			}}
		/>
	</View>
</View>

// stylesheet object
const styles = StyleSheet.create({
	screen: {
		padding: 50,
	},
});
```

To make area scrollable, use <ScrollView> component. However, note that <ScrollView> will render all items in the advance, which can affect performance with very long lists. For 'infinite' lists, use <FlatList data={inputData} renderItem={itemData => <Text>{itemData.item.value}</Text>}> component (note, use keyExtractor for unique key properties).

<Touchable> and its child components allow to listen and respond to touch events.

## Error Handling

### Debugging Logic

To use remote debugger, open Expo menu overlay by pressing Ctrl + M on Android (Cmd + D on iOS) and click on 'Debug JS Remotely'.

A new tab will open in a browser that you can use for debugging. In the browser, go to Sources tab -> choose debuggerWorker.js -> In there, will see your project folder structure. Then, navigate to the file you need to debug and use the browser to set breakpoints. Remember to stop the debugger after you're done.

### Debugging Layout

Open Expo menu overlay and click on 'Toggle Inspector'. This will enable a menu showing styling information about components.

Another option is to install React Native Debugger. Note, you will need to enable Remote Debugging for it to work.











