
Group 40:-
Sreenivasulu Kotakonda
Madhupal Singu


Short Description:-
It's a game like a diceing game, In this application workes according to the different phenominas and rules. In this project or application the battele is inbetween a human and a bot, It all about who's scores reached first exactly 50 that person will won the game.

codes:-
App.json:-
import React, { useState } from 'react';
import { View, Text, TextInput, Button, StyleSheet, Image, ImageBackground } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import img1 from './assets/1.jpeg';
import img2 from './assets/2.jpeg';
import img3 from './assets/3.jpeg';
import img4 from './assets/4.jpeg';
import img5 from './assets/5.jpeg';
import img6 from './assets/6.jpeg';
//const Stack = createStackNavigator();
// Home Screen Component
function HomeScreen({ navigation }) {
  const [playerName, setPlayerName] = useState('');
  return (
    <View style={styles.container}>
      {/* <ImageBackground source={require('./assets/hb.jpeg')} style={styles.background} resizeMode="cover"></ImageBackground> */}
      <Text style={styles.title}>Welcome to the Dice Game!</Text>
      <TextInput
        style={styles.input}
        placeholder="Enter your name"
        value={playerName}
        onChangeText={setPlayerName}
      />
      <Button
        title="Start Game"
        onPress={() => navigation.navigate('Game', { playerName })}
      />
      <ImageBackground source={require('./assets/hb.jpeg')} style={styles.background} resizeMode="cover"></ImageBackground>
      {/* </ImageBackground> */}
    </View>
  );
};
function RulesScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Game Rules</Text>
      <Text style={styles.rulesText}>
        Here are the rules of the game:
        - The game is played using a single dice.
        - Players roll the dice in turns.
        - The first player to reach exactly 50 points wins the game.
        - If a player rolls a '1', their turn is skipped.
        - If a player's score exceeds 50, they must roll exactly the number that would land them on 50 to win.
      </Text>
      <Button title="Go Back" onPress={() => navigation.goBack()} />
    </View>
  );
};

function GameScreen({ route, navigation }) {
  const { playerName } = route.params;
  const targetScore = 50;
  const [playerScore, setPlayerScore] = useState(0);
  const [botScore, setBotScore] = useState(0);
  const [turn, setTurn] = useState('player'); // 'player' or 'bot'

  // Simulate dice roll
  const rollDice = () => {
    return Math.floor(Math.random() * 6) + 1;
  };


  const [message, setMessage] = useState('');
  const [diceImage, setDiceImage] = useState(null);

  const handleRoll = () => {
    const diceValue = rollDice();
    console.log(`Rolled: ${diceValue}, Current Turn: ${turn}`);
    
    const diceImages = [img1, img2, img3, img4, img5, img6];
    setDiceImage(diceImages[diceValue - 1]); // Set the corresponding dice image
  
    if (turn === 'player') {
      const potentialNewScore = playerScore + diceValue;
      if (diceValue === 1 && playerScore !== 49) {
        setMessage("Sorry, you got '1'. You missed the chance.");
        setTurn('bot');
      } else if (potentialNewScore > 50) {
        setMessage("Score exceeds 50. Try next time!");
        setTurn('bot');
      } else if (potentialNewScore === 50) {
        setPlayerScore(potentialNewScore);
        alert(`${playerName} wins!`);
        navigation.goBack(); // Reset or go back to a home screen
      } else {
        setPlayerScore(potentialNewScore);
        setMessage('');
        setTurn('bot');
      }
    } else { // Bot's turn
      const potentialNewScore = botScore + diceValue;
      if (diceValue === 1 && botScore !== 49) {
        setMessage("Bot rolled '1'. Bot misses the chance.");
        setTurn('player');
      } else if (potentialNewScore > 50) {
        setMessage("Bot's score exceeds 50. Try next time!");
        setTurn('player');
      } else if (potentialNewScore === 50) {
        setBotScore(potentialNewScore);
        alert('Bot wins!');
        navigation.goBack();
      } else {
        setBotScore(potentialNewScore);
        setMessage('');
        setTurn('player');
      }
    }
  };
  
  return (

    <View style={styles.container}>
    <ImageBackground source={require('./assets/gb.jpeg')} style={styles.background} resizeMode="cover">
      <Text style={styles.message}>{message}</Text>
      <Text style={styles.gameInfo}>Player: {playerName} - Score: {playerScore}</Text>
      <Text style={styles.gameInfo}>Target: {targetScore}</Text>
      <Text style={styles.gameInfo}>Bot Score: {botScore}</Text>
      
      <View style={styles.gameContainer}>
      <View style={styles.buttonContainer}>
        <Button title="View Rules" onPress={() => navigation.navigate('Rules')} />
      </View>
        {diceImage && <Image source={diceImage} style={styles.diceImage} />}
      <Button title="Roll Dice" onPress={handleRoll} />
    </View>
    </ImageBackground>
    </View>
  );
}


import { ToastAndroid } from 'react-native';

const showFeedback = (message) => {
  ToastAndroid.showWithGravity(
    message,
    ToastAndroid.SHORT,
    ToastAndroid.CENTER
  );
};


// Stack Navigator
const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Game" component={GameScreen} />
        <Stack.Screen name="Rules" component={RulesScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

const styles = StyleSheet.create({
  background: {
    flex: 1,
    width: '100%',
    height: '100%',
    justifyContent: 'center',
    alignItems: 'center'
  },
  buttonContainer: {
    position: 'absolute',
    top: 10,
    left: 10,
    zIndex: 1,
  },
  buttonContainer: {
    position: 'absolute',
    top: -450,
    left: 175,
    zIndex: 1, // Ensure the button appears above other content
  },
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  rulesText: {
    fontSize: 16,
    textAlign: 'justify',
    marginBottom: 20,
  },
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
    color: 'white',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  rulesText: {
    fontSize: 16,
    textAlign: 'justify',
    marginBottom: 20,
  },
  diceImage: {
    width: 100,
    height: 100,
    resizeMode: 'contain'
  },
  gameContainer: {
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20
  },
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    color: 'white',
  },
  title: {
    fontSize: 24,
    marginBottom: 20,
  },
  input: {
    height: 40,
    margin: 12,
    borderWidth: 1,
    padding: 10,
    width: 200,
    textAlign: 'center',
  },
  gameInfo: {
    fontSize: 18,
    marginVertical: 10,
    color: 'white',
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 10,
    backgroundColor: '#f9f9f9',
    alignItems: 'center',
  },
  messageContainer: {
    padding: 10,
    justifyContent: 'center',
    alignItems: 'center'
  },
  message: {
    fontSize: 20,
    color: 'red',
  },
  playerName: {
    fontSize: 20,
    color: 'white',
  },
  targetScore: {
    fontSize: 20,
    color: 'white',
  },
  botScore: {
    fontSize: 20,
    color: 'white',
  }
});
export default App;


, screenshots:-
https://github.com/sreenu2804/my-app/tree/main/assets/ScreenShots


, A link of my shortvideo which is the direct repo link:-
https://github.com/sreenu2804/my-app/assets/ScreenShots/video1443253889.mp4
