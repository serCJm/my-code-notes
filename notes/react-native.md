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

&lt;View> is like a div. It's used for layout and styling.

&lt;View> uses Flexbox to organize its children. A &lt;View> can hold as many child components as you need and it also works with any kind of child component - it can hold &lt;Text> components, other &lt;View>s (for nested containers/ layouts), &lt;Image>s, custom components etc.

To make area scrollable, use &lt;ScrollView> component. Also, if wrapping &lt;ScrollView> with &lt;View>, remember to set flex: 1 on Android to make it scrollable. To style &lt;ScrollView>, use contentContainerStyle prop with flexGrow.

However, note that &lt;ScrollView> will render all items in advance, which can affect performance with very long lists. For 'infinite' lists, use:

```jsx
<FlatList data={inputData} renderItem={itemData => <Text>{itemData.item.value}</Text>}>
//note, use keyExtractor for unique key properties.
```

&lt;Touchable> and its child components allow to listen and respond to touch events.

Text can only be put inside a &lt;Text> component. &lt;Text> components can also be nested inside each other and will also inherit styling. Actually, you can also have nested &lt;View>s inside of a &lt;Text> but that comes with certain caveats/bugs you should watch out for.

Unlike &lt;View>, &lt;Text> does NOT use Flexbox for organizing its content (i.e. the text or nested components). Instead, text inside of &lt;Text> automatically fills a line as you would expect it and wraps into a new line if the text is too long for the available &lt;Text> width.

You can avoid wrapping by setting the numberOfLines prop, possibly combined with ellipsizeMode.

```jsx
Text numberOfLines={1} ellipsizeMode="tail">
  This text will never wrap into a new line, instead it will be cut off like this if it is too lon...
</Text>
```

Also important: When adding styles to a &lt;Text> (no matter if that happens via inline styles or a StyleSheet object), the styles will actually be shared with any nested &lt;Text> components.

This differs from the behavior of &lt;View> (or actually any other component - &lt;Text> is the exception): There, any styles are only applied to the component to which you add them. Styles are never shared with any child component!s

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

### Adding fonts
Add fonts into a dedicated folder (./assets/fonts).

```jsx
import * as Font from "expo-font";
import { AppLoading } from "expo";

function fetchFonts() {
	return Font.loadAsync({
		"open-sans": require("./assets/fonts/OpenSans-Regular.ttf"),
		"open-sans-bold": require("./assets/fonts/OpenSans-Bold.ttf"),
	});
}

export default function App() {
	const [dataLoaded, setDataLoaded] = useState(false);

	if (!dataLoaded) {
		return <AppLoading
				startAsync={fetchFonts}
				onFinish={() => setDataLoaded(true)}
				onError={(err) => console.log(err)}
			></AppLoading>;
	}
}
```

### Adding icons
```jsx
import { Ionicons } from "@expo/vector-icons";

<Ionicons name="md-remove" size={24} color="white"/>
```

### Setting Global Styles
1. Can create a custom component wrapper with the desired styled attached. (i.e. &lt;BodyText>)
2. Have a globally managed StyleSheet that you import into a component and apply where needed.

```jsx
import React from "react";
import { StyleSheet, Text, TextStyle } from "react-native";

interface Props {
	style?: TextStyle;
}

const BodyText: React.FC<Props> = ({ style, children }) => {
	return <Text style={{ ...styles.text, ...style }}>{children}</Text>;
};

export default BodyText;

const styles = StyleSheet.create({
	text: {
		fontFamily: "open-sans",
	},
});
```

### Styling Images
```jsx
import { StyleSheet, View, Image } from "react-native";

const GameOverScreen: React.FC<Props> = () => {
	return (
		<View style={styles.imageContainer}>
			<Image
				source={require("../assets/success.png")}
				style={styles.image}
				resizeMode="contain"
			></Image>
		</View>

	);
};

export default GameOverScreen;

const styles = StyleSheet.create({
	imageContainer: {
		width: 300,
		height: 300,
		borderRadius: 150,
		borderWidth: 3,
		borderColor: "black",
		overflow: "hidden",
		marginVertical: 30,
	},
	image: {
		width: "100%",
		height: "100%",
	},
});
```

Note, to load an image from the web, use source={{uri: 'link'}} and explicitly set width and height (RN is unable to determine the right size).

### Responsive Interfaces

For flexile interfaces, use Dimensions API. Import it from react-native and use its object.

```jsx
const styles = StyleSheet.create({
	button: {
		width: Dimensions.get('window').width / 4,
		// note, can also apply conditional styles in render
		marginTop: Dimensions.get("window").height > 600 ? 20 : 5,
	}
});
```

Dimensions only runs on component render. Thus, if you change orientation, the layout won't update until refresh. If you have a property that needs to orientation layout changes, instead of managing it with Dimensions like above, manage it with state:

```jsx
	const [buttonWidth, setButtonWidth] = useState(
		Dimensions.get("window").width / 4
	);

	useEffect(() => {
		function updateLayout() {
			setButtonWidth(Dimensions.get("window").width / 4);
		}

		Dimensions.addEventListener("change", updateLayout);
		return () => {
			Dimensions.removeEventListener("change", updateLayout);
		};
	}, []);

	// <View style={{ width: buttonWidth }}>
	// 	<Button
	// 		title="Reset"
	// 		color={Colors.accent}
	// 		onPress={resetInputHandler}
	// 	></Button>
	// </View>
```

Note, you can also render different layouts based on the Dimensions state.

### Orientation

You can adjust "locked in" orientation in expo in app.json. Change it to either portrait, landscape, default (supports both). Then you will be able to rotate the screen in an emulator.

To prevent keyboard from covering content, use KeyboardAvoidingView inside ScrollView:
```jsx
<ScrollView>
	<KeyboardAvoidingView
		behavior="position"
		keyboardVerticalOffset={30}
	>
	</KeyboardAvoidingView>
</ScrollView>
```

For layouts that don't depend on width and height but only depend on screen orientation, use ScreenOrientation API.

### Platform

To style based on a platform, use Platform API.

```jsx
const styles = StyleSheet.create({
	button: {
		backgroundColor: Platform.OS === 'android' ? 'green' : 'red',
		borderBottomWidth: Platform.OS === 'ios' ? 2 : 5,
	}
});

// OR
<View style={{...styles.headerBaseStyle, ...Platform.select({ios: styles.headerIOS, android: styles.headerAndroid})}}></View>

// OR
let ButtonComponent = TouchableOpacity;
if (Platform.OS === 'android' && Platform.version >= 21) {
	ButtonComponent = TouchableNativeFeedback;
}
```

Another way to handle different platforms is to use platform specific files by giving either .android or .ios file extensions: Button.android.jsx or Button.ios.jsx. Note, when importing these files, do not provide the extension. Expo will automatically use the right file for each platform.

### Avoiding Notches And Native Buttons

Wrap your top (outer) content with SafeAreaView to prevent screen notches and other native ui elements from covering your app screen space.

## Error Handling

### Debugging Logic

To use remote debugger, open Expo menu overlay by pressing Ctrl + M on Android (Cmd + D on iOS) and click on 'Debug JS Remotely'.

A new tab will open in a browser that you can use for debugging. In the browser, go to Sources tab -> choose debuggerWorker.js -> In there, will see your project folder structure. Then, navigate to the file you need to debug and use the browser to set breakpoints. Remember to stop the debugger after you're done.

### Debugging Layout

Open Expo menu overlay and click on 'Toggle Inspector'. This will enable a menu showing styling information about components.

Another option is to install React Native Debugger. Note, you will need to enable Remote Debugging for it to work.

## Navigation

Install with this command:
```cli
npm install react-navigation

expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

Also, install react-native-screens for better optimization. npm install react-native-screens:
```jsx
// in top component
import { enableScreens } from "react-native-screens";
enableScreens();
```

### Stack Navigation

npm install --save react-navigation-stack

Used to navigate back and forth between screens where screen is stacked on top.

```jsx
// example: https://medium.com/@vdelacou/add-react-navigation-to-react-native-typescript-app-d1cf855b3fe7
// ROOT NAVIGATOR
import { createStackNavigator } from "react-navigation-stack";
import CategoriesScreen from "../screens/CategoriesScreen";
import CategoriesMealScreen from "../screens/CategoryMealsScreen";
import MealDetailScreen from "../screens/MealDetailScreen";
import { createAppContainer } from "react-navigation";

export enum ROUTES {
	Categories = "Categories",
	CategoryMeals = "CategoryMeals",
	MealDetail = "MealDetail",
}

// set screens
const MealNavigator = createStackNavigator(
	{
		[ROUTES.Categories]: {
			screen: CategoriesScreen,
			// note, specific options always win over default
			// navigationOptions: {
			// 	headerTitle: 'some title'
			// }
		},
		[ROUTES.CategoryMeals]: {
			screen: CategoriesMealScreen,
		},
		[ROUTES.MealDetail]: {
			screen: MealDetailScreen,
		},
	},
	{
		defaultNavigationOptions: {
			headerStyle: {
				backgroundColor:
					Platform.OS === "android" ? Colors.primaryColor : "",
			},
			headerTintColor: "white",
		},
	}
);

// always wrap root/most important navigator
export default createAppContainer(MealNavigator);

// SCREEN COMPONENT
import React from "react";
import { StyleSheet, Text, View, Button } from "react-native";
import { CATEGORIES } from "../data/data";
import Category from "../models/category";
import { NavigationScreenComponent } from "react-navigation";
import {
	NavigationStackScreenProps,
	NavigationStackOptions,
} from "react-navigation-stack";

type Props = {
	navigation: NavigationStackProp
}

type Params = {};

type ScreenProps = {};

const CategoriesMealScreen: NavigationScreenComponent<Params, ScreenProps> = (
	props: Props
) => {
	const catId = props.navigation.getParam("categoryId");

	const selectedCategory: Category | undefined = CATEGORIES.find(
		(cat) => cat.id === catId
	);
	return (
		<View style={styles.screen}>
			<Text>The CategoriesMealScreens Screen!</Text>
			<Text>{selectedCategory?.title}</Text>
			<Button
				title="Go To Details!"
				onPress={() =>
					// alternative syntax
					// props.navigation.navigate('SomeIdentifier');
					// can also use push, pop, replace, popToTop, goBack
					props.navigation.navigate({ routeName: "MealDetail" })
				}
			></Button>
			<Button
				title="Go Back!"
				onPress={() => props.navigation.goBack()}
			></Button>
		</View>
	);
};

CategoriesMealScreen.navigationOptions = (
	navigationData: NavigationStackScreenProps
): NavigationStackOptions => {
	const catId = navigationData.navigation.getParam("categoryId", "None");
	const selectedCategory: Category | undefined = CATEGORIES.find(
		(cat) => cat.id === catId
	);
	return {
		headerTitle: selectedCategory?.title,
	};
};

```

When defining a navigator, you can also add navigationOptions to it:
```jsx
    const SomeNavigator = createStackNavigator({
        ScreenIdentifier: SomeScreen
    }, {
        navigationOptions: {
            // You can set options here!
            // Please note: This is NOT defaultNavigationOptions!
        }
    });
```
Don't mistake this for the defaultNavigationOptions which you could also set there (i.e. in the second argument you pass to createWhateverNavigator()).

The navigationOptions you set on the navigator will NOT be used in its screens! That's the difference to defaultNavigationOptions - those option WILL be merged with the screens.

These options become important once you use the navigator itself as a screen in some other navigator - for example if you use some stack navigator (created via createStackNavigator()) in a tab navigator (e.g. created via createBottomTabNavigator()).

### Navigation Buttons

1. Install react-navigation-header-buttons package
2. Use the button:
```jsx
import React from "react";
import { Platform } from "react-native";
import { HeaderButton } from "react-navigation-header-buttons";
import { Ionicons } from "@expo/vector-icons";
import { Colors } from "../constants/Colors";

interface Props {
	title: string;
}

const CustomHeaderButton = (props: Props) => {
	return (
		<HeaderButton
			{...props}
			IconComponent={Ionicons}
			iconSize={23}
			color={Platform.OS === "android" ? "white" : Colors.primaryColor}
		></HeaderButton>
	);
};

export default CustomHeaderButton;

// then use in a component:
MealDetailScreen.navigationOptions = (
	navigationData: NavigationStackScreenProps
): NavigationStackOptions => {
	const mealId = navigationData.navigation.getParam("mealId");
	const selectedMeal = MEALS.find((meal) => meal.id === mealId);
	return {
		headerTitle: selectedMeal?.title,
		headerRight: () => (
			<HeaderButtons HeaderButtonComponent={CustomHeaderButton}>
				<Item
					title="Favorite"
					iconName="ios-star"
					onPress={() => {}}
				></Item>
			</HeaderButtons>
		),
	};
};
```

### Tabs Navigation
```jsx
const MealsFavTabNavigator = createBottomTabNavigator({
	Meals: {
		screen: MealNavigator,
		navigationOptions: {
			tabBarIcon: (tabInfo) => {
				return (
					<Ionicons
						name="ios-restaurant"
						size={25}
						color={tabInfo.tintColor}
					></Ionicons>
				);
			},
		},
	},
	Favorites: {
		screen: FavoritesScreen,
		navigationOptions: {
			tabBarLabel: "Favorites!",
			tabBarIcon: (tabInfo) => {
				return (
					<Ionicons
						name="ios-star"
						size={25}
						color={tabInfo.tintColor}
					></Ionicons>
				);
			},
		},
	},
});
```

Fot Android native looking tabs, use packages react-navigation-material-bottom-tabs and react-native-paper.

### Drawer Navigation
```jsx
const MainNavigator = createDrawerNavigator({
	MealsFavs: {
	screen: MealsFavTabNavigator,
	navigationOptions: {
		drawerLabel: 'Meals'
	}
	},
	Filters: {
		screen: FiltersNavigator,
		navigationOptions: {
			drawerLabel: 'Filters'
		}
		,
}}, {
	contentOptions: {
		activeTintColor: Colors.accentColor,
		labelStyle: {
			fontFamily: 'open-sans-bold'
		}
	}
});
```

### Passing Data Between Component and navigationOptions
```jsx
const saveFilters = useCallback(() => {
		const appliedFilters = {
			glutenFree: isGlutenFree,
			lactoseFree: isLactoseFree,
			vegan: isVegan,
			isVegeterean: isVegeterean,
		};
	}, [isGlutenFree, isLactoseFree, isVegan, isVegeterean]);

	useEffect(() => {
		navigation.setParams({ save: saveFilters });
	}, [saveFilters]);

type navOptions = NavigationStackScreenProps & NavigationDrawerScreenProps;

FiltersScreen.navigationOptions = (
	navData: navOptions
): NavigationStackOptions => {
	return {
		headerTitle: "Filter Meals",
		headerLeft: () => (
			<HeaderButtons HeaderButtonComponent={CustomHeaderButton}>
				<Item
					title="Menu"
					iconName="ios-menu"
					onPress={() => navData.navigation.toggleDrawer()}
				></Item>
			</HeaderButtons>
		),
		headerRight: () => (
			<HeaderButtons HeaderButtonComponent={CustomHeaderButton}>
				<Item
					title="Save"
					iconName="ios-save"
					onPress={navData.navigation.getParam("save")}
				></Item>
			</HeaderButtons>
		),
	};
};
```

### Accessing Navigation Outside of Navigator
```jsx
const NavigationContainer = (props: Props) => {
	const isAuth = useSelector((state: RootState) => !!state.auth.token);
	const navRef: React.RefObject<NavigationContainerComponent> | null = useRef() as React.RefObject<NavigationContainerComponent> | null;
	useEffect(() => {
		if (!isAuth && navRef) {
			navRef?.current?.dispatch(
				NavigationActions.navigate({ routeName: "Auth" })
			);
		}
	}, [isAuth]);
	return <ShopNavigator ref={navRef}></ShopNavigator>;
};

export default NavigationContainer;

const styles = StyleSheet.create({});
```

```jsx
// ShopNavigator
type NavContainerParams = {};

type NavContainerProps = {
	ref: React.RefObject<NavigationContainerComponent> | null;
};

export default createAppContainer<NavContainerParams, NavContainerProps>(
	MainNavigator
);
```

## Store Management

### Setting up Redux

```cli
npm install redux react-redux
```

Create store folder => reducers + actions folders

Create a reducer:
```jsx
// import Meal from "../models/meal";

// export type MealState = {
// 	meals: Meal[];
// 	filteredMeals: Meal[];
// 	favoriteMeals: Meal[];
// };
import { MEALS } from "../../data/data";
import { MealsActionTypes, TOGGLE_FAVORITE } from "../actions/types";
import { MealState } from "../type";

const initialState: MealState = {
	meals: MEALS,
	filteredMeals: MEALS,
	favoriteMeals: [],
};

export const mealsReducer = (
	state = initialState,
	action: MealsActionTypes
): MealState => {
	switch (action.type) {
		case TOGGLE_FAVORITE:
			const existingIndex = state.favoriteMeals.findIndex(
				(meal) => meal.id === action.mealId
			);
			if (existingIndex >= 0) {
				const updatedFavMeals = [...state.favoriteMeals];
				updatedFavMeals.splice(existingIndex, 1);
				return { ...state, favoriteMeals: updatedFavMeals };
			} else {
				const meal = state.meals.find(
					(meal) => meal.id === action.mealId
				);
				if (meal)
					return {
						...state,
						favoriteMeals: state.favoriteMeals.concat(meal),
					};
			}
		default:
			return state;
	}
};
```

Create store
```jsx
import { createStore, combineReducers } from "redux";
import { mealsReducer } from "./store/reducers";
import { Provider } from "react-redux";

const rootReducer = combineReducers({
	meals: mealsReducer,
});

export type RootState = ReturnType<typeof rootReducer>;

const store = createStore(rootReducer);

return (
		<Provider store={store}>
			<MealsNavigator></MealsNavigator>
		</Provider>
	);
```

In the component, use useSelector:
```jsx
const availableMeals = useSelector((state: RootState) => state.meals.meals);
```

To forward state store to navigationOptions either set parameters in the component using setParams and useEffect (however, can lead to stale initial state) or pass forward from a screen from which coming to that screen.

Add an action:
```jsx
// export const TOGGLE_FAVORITE = "TOGGLE_FAVORITE";

// interface ToggleFavoriteAction {
// 	type: typeof TOGGLE_FAVORITE;
// 	mealId: string;
// }

// interface DeleteMessageAction {
// 	type: typeof DELETE_MESSAGE;
// 	meta: {
// 		timestamp: number;
// 	};
// }

// export type MealsActionTypes = ToggleFavoriteAction;

import { ToggleFavoriteAction, TOGGLE_FAVORITE } from "./types";

export const toggleFavorite = (id: string): ToggleFavoriteAction => {
	return { type: TOGGLE_FAVORITE, mealId: id };
};
```

Dispatch an action:
```jsx
const dispatch = useDispatch();

const toggleFavoriteHandler = useCallback(() => {
	dispatch(toggleFavorite(mealId));
}, [dispatch, mealId]);

useEffect(() => {
	props.navigation.setParams({ toggleFav: toggleFavoriteHandler });
}, [toggleFavoriteHandler]);
```

## Using Device Features

### Using Camera

Either use ImagePicker or Camera expo components. Note, these need to be installed separately. (expo install expo-image-picker)

Note, to access camera on iOS, need to access it first. Use Permissions expo package. (expo install expo-permissions)

```jsx
import React, { useState } from "react";
import { Button, StyleSheet, Text, View, Image, Alert } from "react-native";
import * as ImagePicker from "expo-image-picker";
import * as Permissions from "expo-permissions";
import { Colors } from "../assets/Colors";

interface Props {
	onImageTake: (imageUri: string) => void;
}

const ImagePickerComponent = (props: Props) => {
	const [pickedImage, setPickedImage] = useState("");
	const verifyPermissions = async () => {
		const result = await Permissions.askAsync(
			Permissions.CAMERA,
			Permissions.CAMERA_ROLL
		);
		if (result.status !== "granted") {
			Alert.alert(
				"No Permissions Found",
				"Need permissions to access camera.",
				[{ text: "OK" }]
			);
			return false;
		}
		return true;
	};
	const takeImageHandler = async () => {
		const hasPermissions = await verifyPermissions();
		if (!hasPermissions) return;
		const image = await ImagePicker.launchCameraAsync({
			allowsEditing: true,
			aspect: [16, 9],
			quality: 0.5,
		});
		if (!image.cancelled) {
			setPickedImage(image.uri);
			props.onImageTake(image.uri);
		}
	};
	return (
		<View style={styles.imagePicker}>
			<View style={styles.imagePreview}>
				{!pickedImage ? (
					<Text>No image picked yet.</Text>
				) : (
					<Image
						style={styles.image}
						source={{ uri: pickedImage }}
					></Image>
				)}
			</View>
			<Button
				title="Take Image"
				color={Colors.PRIMARY}
				onPress={takeImageHandler}
			></Button>
		</View>
	);
};

export default ImagePickerComponent;
```

### Storing Image on Filesystem

To move files from one directory to another, use file-system expo package.

```jsx
import * as FileSystem from "expo-file-system";
import { ThunkAction } from "redux-thunk";
import { RootState } from "../App";
import { ADD_PLACE, PlacesActionTypes } from "./types";

export const addPlace = (
	title: string,
	image: string
): ThunkAction<void, RootState, unknown, PlacesActionTypes> => {
	return async (dispatch) => {
		const fileName = image.split("/").pop() || "";
		const newPath = FileSystem.documentDirectory
			? FileSystem.documentDirectory + fileName
			: `${new Date().toISOString()}.jpg`;
		try {
			await FileSystem.moveAsync({
				from: image,
				to: newPath,
			});
		} catch (e) {
			console.log(e);
			throw e;
		}
		return dispatch({
			type: ADD_PLACE,
			placeData: { title, image: newPath },
		});
	};
};
```

### Using SQLite For Device Storage

Use expo sqlite package.

```jsx
import * as SQLite from "expo-sqlite";

const db = SQLite.openDatabase("places.db");

export const init = () => {
	const promise = new Promise((res, rej) => {
		db.transaction((tx) => {
			tx.executeSql(
				"CREATE TABLE IF NOT EXISTS places (id INTEGER PRIMARY KEY NOT NULL, title TEXT NOT NULL, imageUri TEXT NOT NULL, address TEXT NOT NULL, lat REAL NOT NULL, lng REAL NOT NULL);",
				[],
				() => {
					res();
				},
				(_, err) => {
					rej(err);
					return false;
				}
			);
		});
	});
	return promise;
};

export const insertPlace = (
	title: string,
	imageUri: string,
	address: string,
	lat: number,
	lng: number
) => {
	const promise = new Promise((res, rej) => {
		db.transaction((tx) => {
			tx.executeSql(
				"INSERT INTO places (title, imageUri, address, lat, lng) VALUES (?, ?, ?, ?, ?);",
				[title, imageUri, address, lat, lng],
				(_, result) => {
					res(result);
				},
				(_, err) => {
					rej(err);
					return false;
				}
			);
		});
	});
	return promise;
};

export const fetchPlaces = () => {
	const promise = new Promise((res, rej) => {
		db.transaction((tx) => {
			tx.executeSql(
				"SELECT * FROM places;",
				[],
				(_, result) => {
					res(result);
				},
				(_, err) => {
					console.log("Fetch places error");
					rej(err);
					return false;
				}
			);
		});
	});
	return promise;
};

interface ISQLResultSet extends SQLite.SQLResultSet {
	rows: {
		_array: Place[];
		length: number;
		item(index: number): any;
	};
}

export const getPlaces = (): ThunkAction<
	void,
	RootState,
	unknown,
	PlacesActionTypes
> => {
	return async (dispatch) => {
		try {
			const dbResult = (await fetchPlaces()) as ISQLResultSet;
			dispatch({ type: SET_PLACES, places: dbResult.rows._array });
		} catch (e) {
			console.log(e);
			throw e;
		}
	};
};
```








