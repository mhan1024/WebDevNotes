# React Native
Used for building cross-platform apps (web, iOS, Android)

## Environment setup & basic commands
- Create app (project):
  ```
  npx @react-native-community/cli init APPNAME
  ```

## Basic components
- Be sure to import from react-native
  ```javascript
  import { Text, View } from 'react-native';
  ```
- ```<View></View>```: a container that supports layout 
- ```<Text></Text>```: used for text and can be used to handle touch events
- ```<Image></Image>```: displays images
- ```<ScrollView></ScrollView>```: a scrollable container for multiple components and views
  - Scrollable items can be heterogeneous
  - ```horizontal```: a prop that sets scroll to vertical or horizontal 
- ```<TextInput></TextInput>```: input text fields
  - ```onChangeText```: a prop that takes a function to be called every time the text changes
  - ```onSubmitEditing```: a prop that takes a function to be called when the text is submitted
 
## List Views
- FlatList: a component displaying a scrolling list of changing, but similarly structured data
  - Best for long lists of data, where the number of items change over time
  - Only renderes elements that are currently showing on screen
  - Requires 2 props: data and renderItem
      - ```data```: source of information
      - ```renderItem```: a function for defining how to display each item in ```data```

## Flexbox layout
- flexDirection: Determines the primary axis ('row' or 'column')
- justifyContent: Aligns children along the primary axis
- alignItems: Aligns children along the secondary axis
- flex: Determines how a component grows or shrinks

## Styles
```javascript
<View style={styles.container}>
  <Text style={{ color: 'blue', fontSize: 20 }}>Styled Text</Text>
</View>
```

## Platform-Specific & Native-Specific code
- Platform module: helps identify what device the app is running on
  ```javascript
  import {Platform, StyleSheet} from 'react-native';

  const styles = StyleSheet.create({
    height: Platform.OS === 'ios' ? 200 : 100,
  });

  // Conditional logic for platform-specific values
  Platform.select({
    ios: ...,
    android: ...,
  });
  ```
- Native module: a module that needs to be shared between web and Native, but it has no Android/iOS differences

## Router
- Simple names/no notation: static routes for regular files and directory names
- Square brackets notation: dynamic routes where the name of the route is a parameter that can be used when rendering the page
  - EX: ```[userName].tsx``` can be matched to ```/mhan``` or some other username
  - ```useLocalSearchParams```: a hook that can access the file parameter from inside the page
- Parentheses notation: a group of routes
- ```_layout.tsx```: files that are not pages but define how route groups inside a directory relate to each other
  - Layout routes are rendered before the page

## Router Layouts
- ```_layout.tsx```: root layout and the entry point for navigation
  - Can be used for initialization code
- Stack navigator: controls how pages are pushed and popped off a stack
  - So, one screen is displayed at a time
  - New screens are added on top of the current one (LIFO)
  - Let users go back to the previous screen
  - In the ```_layout.tsx```, return a Stack component: ```<Stack />```
- Tab navigator: all routes in the directory will be treated as tabs
  - Let users switch between different pages at any time
  - In the ```_layout.tsx```, return a Tab component: ```<Tabs />```
  - Syntax:
    ```html
    <Tabs>
      <Tabs.Screen
        name="TAB NAME"
        options={{
          title: 'TAB TITLE'
          tabBarIcon: ...
        }}
      />
    </Tabs>
    ```
- Slot: a placeholder layout without a navigator
  - Useful for adding a header or footer around a current route
  - In the ```_layout.tsx```, return a Slot component: ```<Slot />```
 
## Navigation between pages (Expo Router)
- ```useRouter```: a hook that gives you a way to navigat programmatically, instead of using links
  ```javascript
  import { useRouter } from 'expo-router';
  import { Button } from 'react-native';
  
  export default function Home() {
    const router = useRouter();
  
    return <Button title="Go to About" onPress={() => router.navigate('/about')} />;
  }
  ```
  - ```router.push```: explicitly push onto stack
  - ```router.back```: go back to the previous page
  - ```router.replace```: replace the current page on stack
- Links & buttons
  - ```<Link />```: a component similar to the ```<a>``` tag and handles simple navigation
    - Used for user tap actions
    - ```<Pressable></Pressable>```: a component that lets you detect and respond to user touches
- Relative routes: navigation based on the current file's location
  - ```\```: root
  - ```.```: current folder
  - ```..```: parent folder
- Dynamic routes: can be linked to with a full URL or passing a ```params``` object
  - Full URL:
    ```html
    <Link
      href="/user/bacon">
      View user (id inline)
    </Link>
    ```
  - ```params``` object:
    ```html
    <Link
      href={{
          pathname: '/user/[id]',
          params: { id: 'bacon' }
      }}
    >
      View user (id inline)
    </Link>
    ```
- Redirect to another route: ```<Redirect href="/">```
- Prefetch: a prop that loads a route's code/data before the user navigates to it
  - Syntax: ```<Link href="/about" prefetch />```
  - Allows for faster navigation
- Deep links: a URL that opens a specific screen inside the app
  - Syntax: ```scheme://path?query=params```

## Firebase Authentication
- Prequisite: @react-native-firebase/app
  - ```npm install @react-native-firebase/app```
  - ```npm install @react-native-firebase/auth```
  - ```cd ios/ && pod install```: run this to install native iOS dependencies
- ```onAuthStateChanged```: an automatic listener that tells you whether your users are currently signed-in or signed-out
- ```createUserWithEmailAndPassword```: registers the user and signs them in
- ```signInWithEmailAndPassword```: signs the user into an existing account
- ```signOut```: signs user out
- Apple sign-in:
  - ```react-native-apple-authentication```: library that contains the pre-rendered button for Apple sign-in
  ```javascript
  import { AppleAuthProvider, getAuth, signInWithCredential } from '@react-native-firebase/auth';
  import { appleAuth } from '@invertase/react-native-apple-authentication';
  
  async function onAppleButtonPress() {
    // Start the sign-in request
    const appleAuthRequestResponse = await appleAuth.performRequest({
      requestedOperation: appleAuth.Operation.LOGIN,
      // As per the FAQ of react-native-apple-authentication, the name should come first in the following array.
      // See: https://github.com/invertase/react-native-apple-authentication#faqs
      requestedScopes: [appleAuth.Scope.FULL_NAME, appleAuth.Scope.EMAIL],
    });
  
    // Ensure Apple returned a user identityToken
    if (!appleAuthRequestResponse.identityToken) {
      throw new Error('Apple Sign-In failed - no identify token returned');
    }
  
    // Create a Firebase credential from the response
    const { identityToken, nonce } = appleAuthRequestResponse;
    const appleCredential = AppleAuthProvider.credential(identityToken, nonce);
  
    // Sign the user in with the credential
    return signInWithCredential(getAuth(), appleCredential);
  }
  ```
  - When a user deletes their account, you have to revoke their account token
  ```javascript
  import { getAuth, revokeToken } from '@react-native-firebase/auth';
  import { appleAuth } from '@invertase/react-native-apple-authentication';
  
  async function revokeSignInWithAppleToken() {
    // Get an authorizationCode from Apple
    const { authorizationCode } = await appleAuth.performRequest({
      requestedOperation: appleAuth.Operation.REFRESH,
    });
  
    // Ensure Apple returned an authorizationCode
    if (!authorizationCode) {
      throw new Error('Apple Revocation failed - no authorizationCode returned');
    }
  
    // Revoke the token
    return revokeToken(getAuth(), authorizationCode);
  }
  ```
- Google sign-in: you need to initialize the Google SDK and configure it
  ```javascript
  import { GoogleSignin } from '@react-native-google-signin/google-signin';
  import { Button } from 'react-native';
  
  GoogleSignin.configure({
    webClientId: '',
  });

  function GoogleSignIn() {
    return (
      <Button
        title="Google Sign-In"
        onPress={() => onGoogleButtonPress().then(() => console.log('Signed in with Google!'))}
      />
    );
  }
  ```
  - Implement ```onGoogleButtonPress```:
    ```javascript
    import { GoogleAuthProvider, getAuth, signInWithCredential } from '@react-native-firebase/auth';
    import { GoogleSignin } from '@react-native-google-signin/google-signin';
    
    async function onGoogleButtonPress() {
      // Check if your device supports Google Play
      await GoogleSignin.hasPlayServices({ showPlayServicesUpdateDialog: true });
      // Get the users ID token
      const signInResult = await GoogleSignin.signIn();
    
      // Try the new style of google-sign in result, from v13+ of that module
      idToken = signInResult.data?.idToken;
      if (!idToken) {
        // if you are using older versions of google-signin, try old style result
        idToken = signInResult.idToken;
      }
      if (!idToken) {
        throw new Error('No ID token found');
      }
    
      // Create a Google credential with the token
      const googleCredential = GoogleAuthProvider.credential(signInResult.data.idToken);
    
      // Sign-in the user with the credential
      return signInWithCredential(getAuth(), googleCredential);
    }
    ```
