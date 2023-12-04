# Little Lemon Food Ordering App

- The application is a React Native Expo Food app.
- Users will be capable of signing up on the Little Lemon restaurant app.
- Users will have to go through a registration process.
- Once they successfully complete that phase, they are redirected to a home screen.
- Home screen will represent the landing screen after completing the onboarding flow, displaying a header, a banner with a search bar and a list of menu items where a user can filter each categories.
- User can also customize their name, email, photo and and other user preferences through a Profile Screen.
- Profile screen also contains four checkboxes enable specific email notifications, like order status, password changes,special offers, and newsletters.
- Use AsyncStorage module to preserve the chosen preferences even when the user quits the application
- When clicking the Logout button, user will redirect back to login page, clearing all data saved from Profile.
- Use SQLite Database to populate, query and filter menu items.

## Table of contents

- [Overview](#overview)
  - [How to use the project](#how-to-use-the-project)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Useful resources](#useful-resources)
- [Author](#author)

## Overview

### How to use the project

##### npm install && npm start
##### Then, a QR Code wil appear on your terminal.
##### On IOS Scan QR code through Camera app.
##### On Android : Scan QR code through Expo Go app.

##### You can also scan this [QR CODE](https://expo.dev/@marventures/little-lemon-app) to view the project. 

### Screenshot
![final_mockup](https://user-images.githubusercontent.com/108392678/217717918-a6f83c94-c1ab-4796-903e-388b9a67cdd9.jpg)
![Onboarding](https://user-images.githubusercontent.com/108392678/217715066-19026169-ab51-450e-b21c-cc925940d03e.jpg)
![Profile and Home](https://user-images.githubusercontent.com/108392678/217715079-d66eb960-f5cf-4cdf-8f33-b45b320fca7e.jpg)


## My process

### Built with

- [React Native](https://reactnative.dev/docs/environment-setup) - React Native app built with expo
- [SQLite](https://docs.expo.dev/versions/latest/sdk/sqlite/) - For storing restaurant's menu items.
- [AsyncStorage](https://react-native-async-storage.github.io/async-storage/docs/api/) - For storing user preferences.
- [StyleSheet](https://reactnative.dev/docs/stylesheet) - For styles



Here is a code snippet:

```jsx
const [profile, setProfile] = useState({
  firstName: "",
  lastName: "",
  email: "",
  phoneNumber: "",
  orderStatuses: false,
  passwordChanges: false,
  specialOffers: false,
  newsletter: false,
  image: "",
});
const [data, setData] = useState([]);
const [searchBarText, setSearchBarText] = useState("");
const [query, setQuery] = useState("");
const [filterSelections, setFilterSelections] = useState(
  sections.map(() => false)
);

const fetchData = async () => {
  try {
    const response = await fetch(API_URL);
    const json = await response.json();
    const menu = json.menu.map((item, index) => ({
      id: index + 1,
      name: item.name,
      price: item.price.toString(),
      description: item.description,
      image: item.image,
      category: item.category,
    }));
    return menu;
  } catch (error) {
    console.error(error);
  } finally {
  }
};

useEffect(() => {
  (async () => {
    let menuItems = [];
    try {
      await createTable();
      menuItems = await getMenuItems();
      if (!menuItems.length) {
        menuItems = await fetchData();
        saveMenuItems(menuItems);
      }
      const sectionListData = getSectionListData(menuItems);
      setData(sectionListData);
      const getProfile = await AsyncStorage.getItem("profile");
      setProfile(JSON.parse(getProfile));
    } catch (e) {
      Alert.alert(e.message);
    }
  })();
}, []);
```


