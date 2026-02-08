# Sara Voice Assistant - Conversation Flow Documentation

## Overview
Sara is a voice-first AI assistant designed specifically for blind and visually impaired users to navigate public transit systems. The assistant provides hands-free interaction through speech recognition and text-to-speech synthesis, enabling users to plan routes, check information, and receive step-by-step navigation guidance.

## Core Design Principles

### 1. Accessibility-First Design
- **Voice-Only Interface**: All interactions happen through speech, no visual elements required
- **Clear Audio Feedback**: Every action provides immediate audio confirmation
- **Natural Language Processing**: Users can speak naturally without memorizing specific commands
- **Conversational Context**: Sara maintains conversation state to provide relevant responses

### 2. Multilingual Support
Sara supports multiple languages with contextual responses:
- **English** (Default)
- **French** 
- **Spanish**

Language can be changed mid-conversation using voice commands.

## Conversation States and Flow

### State Machine Overview
Sara operates using a finite state machine with the following primary states:

```
IDLE → INTRO → [Various Conversation Paths] → COMPLETION
```

### Primary Conversation States

1. **IDLE**: Initial state, waiting for initialization
2. **INTRO**: Sara introduces herself and provides environmental context
3. **AWAITING_DESTINATION**: Waiting for user to specify where they want to go
4. **AWAITING_TRANSPORT**: Asking user to choose transport type
5. **AWAITING_PREFERENCES**: Gathering user preferences for route selection
6. **READY_TO_START**: Route selected, waiting for user to begin journey
7. **JOURNEY_ACTIVE**: Actively providing navigation guidance
8. **PROFILE_MANAGEMENT**: Managing user profile and settings
9. **NOTIFICATIONS**: Checking and managing notifications
10. **GAMES_MODE**: Engaging in gamification activities
11. **LANGUAGE_SELECTION**: Changing system language

## Initial Conversation Flow

### 1. Sara's Introduction
When the voice assistant starts, Sara automatically provides:

```
"Hi this is Sara... your AI Agent..

You are in [LOCATION].

The temperature is [TEMPERATURE]. Air quality index is [AIR_QUALITY]. 
Humidity is [HUMIDITY].

You've saved [CO2_AMOUNT] of CO₂ this week..

Where do you want to go?"
```

**Environmental Context Provided:**
- Current location (e.g., "Toronto")
- Weather conditions (temperature, humidity)
- Air quality index
- User's weekly CO₂ savings from eco-friendly transport

## Navigation Conversation Pattern

### Route Planning Dialog Flow

**Step 1: Destination Request**
```
Sara: "Where do you want to go?"
User: "I want to go from Union Station to CN Tower"
     OR "Take me to the airport"
     OR "I need to get to the hospital"
```

**Step 2: Transport Selection**
```
Sara: "Okay! You want to go from [ORIGIN] to [DESTINATION]. 
      Which type of transport would you like to take?
      You can choose Bus, Train, or MRT/LRT."

User: "Bus" OR "Train" OR "MRT" OR "I prefer the bus"
```

**Step 3: Route Options**
```
Sara: "Here are the suggested routes for [TRANSPORT] transport...

      Route 1: 5.8 kilometers, estimated travel time 24 minutes. 
      You will arrive at 8:38 PM. Cost is $1.50. 
      CO₂ saved compared to driving: 1.2 kg.

      Route 2: 6.2 kilometers, estimated travel time 22 minutes. 
      You will arrive at 8:36 PM. Cost is $1.20. CO₂ saved: 1.0 kg.

      Route 3: 5.5 kilometers, estimated travel time 26 minutes. 
      You will arrive at 8:40 PM. Cost is $1.80. CO₂ saved: 1.3 kg.

      Do you want me to recommend routes that save more CO₂, 
      or cheaper routes, or do you have any preferences like departure time?"
```

**Step 4: Preference Selection**
```
User: "I want the cheapest route"
     OR "Show me the fastest option"
     OR "I prefer the most eco-friendly route"
     OR "I want to leave at 3 PM"
```

**Step 5: Journey Initiation**
```
Sara: "Perfect! I've selected Route 2 for you. 
      Are you ready to start your journey? Say 'Yes' to begin navigation."

User: "Yes" OR "Start navigation" OR "Let's go"
```

## Key User Commands and Keywords

### Navigation Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| Go to location | "I want to go to Union Station" | "go", "want", "union station" |
| From-to journey | "Take me from here to CN Tower" | "from", "to", "cn tower" |
| Transportation choice | "I prefer the bus" | "bus", "train", "mrt", "prefer" |
| Route preferences | "Show me the cheapest option" | "cheapest", "fastest", "eco-friendly" |
| Start journey | "Let's begin" | "yes", "start", "begin", "let's go" |
| Update location | "I am now at the bus stop" | "now at", "arrived", "reached" |

### Profile Management Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| View profile | "Check my profile" | "profile", "check", "show profile" |
| Edit profile | "I want to edit my name" | "edit", "change", "update" |
| Biometric verification | "Verify my disability status" | "verify", "biometric", "disability" |

### Information Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| Check notifications | "Show my notifications" | "notifications", "messages", "updates" |
| View trip history | "Show my past trips" | "trips", "history", "past" |
| Environmental info | "What's the weather?" | "weather", "temperature", "air quality" |

### Gaming Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| Access games | "I want to play games" | "games", "play", "entertainment" |
| CO₂ Clicker | "Play CO₂ clicker" | "co2 clicker", "clicker", "tap" |
| Trivia | "Start trivia" | "trivia", "quiz", "questions" |
| Check badges | "Show my badges" | "badges", "achievements", "progress" |

### Language Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| Change language | "Change language" | "language", "change language" |
| Specific language | "Switch to French" | "french", "spanish", "english" |

### General Commands
| **Intent** | **Example User Input** | **Keywords Detected** |
|------------|----------------------|----------------------|
| End conversation | "Thank you" | "thank", "bye", "goodbye" |
| Help | "What can you do?" | "help", "what can", "commands" |
| Repeat | "Say that again" | "repeat", "again", "what" |
| Initialize | "Start over" | "initialize", "start", "begin" |

## Location Recognition

### Predefined Locations
Sara recognizes common locations and landmarks:

- **Transit Hubs**: "Union Station", "subway station", "bus stop"
- **Landmarks**: "CN Tower", "downtown", "airport"
- **Services**: "hospital", "library", "shopping mall"
- **Educational**: "university", "school"
- **Relative**: "here", "my current location", "nearby"

### Location Extraction Pattern
Sara can understand various formats:
- "I want to go to [DESTINATION]"
- "Take me from [ORIGIN] to [DESTINATION]"
- "Navigate to [LOCATION]"
- "How do I get to [PLACE]?"

## Journey Guidance Flow

### Active Navigation States

**1. At Bus Stop**
```
Sara: "You should be at the bus stop now. 
      Bus 34 should arrive in 3 minutes. 
      Let me know when you see or hear the bus approaching."

User: "The bus is here" OR "Bus arrived"
```

**2. On Transportation**
```
Sara: "Great! You're now on Bus 34. 
      The journey to your destination will take 18 minutes. 
      I'll let you know when to get off."

User: "How much longer?" OR "Where are we?"
```

**3. Final Approach**
```
Sara: "You'll be getting off at the next stop. 
      After you get off, walk 50 meters north to reach CN Tower."

User: "I'm off the bus" OR "I got off"
```

**4. Arrival**
```
Sara: "Perfect! You should now be at CN Tower. 
      You've completed your journey and saved 1.2 kg of CO₂! 
      Would you like me to help you with anything else?"
```

## Error Handling and Clarification

### Common Clarification Patterns

**Unrecognized Location**
```
Sara: "I didn't recognize that location. 
      Could you tell me a nearby landmark or major intersection?"
```

**Ambiguous Transport**
```
Sara: "I heard you mention transport, but could you specify: 
      Bus, Train, or MRT?"
```

**Missing Information**
```
Sara: "I need to know where you're starting from. 
      Are you at your current location, or somewhere else?"
```

## Accessibility Features

### Audio Design
- **Clear Pronunciation**: All responses use simple, clear language
- **Structured Information**: Information presented in logical order
- **Confirmation Loops**: Important actions require user confirmation
- **Progress Updates**: Regular updates during long processes

### Error Recovery
- **Graceful Degradation**: If speech recognition fails, Sara asks for repetition
- **Context Preservation**: Previous conversation context is maintained
- **Alternative Phrasing**: Sara suggests different ways to phrase requests

## Integration Points

### Backend API Integration
The voice assistant integrates with several backend services:

1. **Assistant Service** (`/api/assistant/query`)
   - Processes natural language input
   - Maintains conversation state
   - Returns structured responses

2. **Maps Service** 
   - Provides routing information
   - Real-time transit data
   - Location geocoding

3. **User Profile Service**
   - Manages user preferences
   - Stores accessibility settings
   - Tracks usage statistics

### Voice Technology
- **Speech Recognition**: Web Speech API (webkitSpeechRecognition)
- **Text-to-Speech**: Web Speech Synthesis API
- **Language Support**: Configurable voice selection
- **Offline Fallback**: Basic functionality without network

## Best Practices for Conversation Design

### For Users
1. **Speak Naturally**: No need to memorize specific commands
2. **Be Specific**: Include key details like origins, destinations, preferences
3. **Confirm Actions**: Always confirm when Sara asks for verification
4. **Use Context**: Build on previous conversation rather than starting over

### For Developers
1. **Maintain State**: Preserve conversation context across interactions
2. **Provide Feedback**: Always acknowledge user input
3. **Error Gracefully**: Handle speech recognition errors smoothly
4. **Test Accessibility**: Verify all features work without visual interface

## Future Enhancements

### Planned Features
- **Real-time Updates**: Integration with live transit APIs
- **Smart Suggestions**: AI-powered route recommendations
- **Voice Customization**: Multiple voice options and speaking speeds
- **Contextual Awareness**: Location-based automatic suggestions
- **Multi-modal Fallback**: SMS/text alternatives when needed

This conversation flow documentation ensures that Sara provides a comprehensive, accessible, and intuitive experience for blind and visually impaired users navigating public transportation systems.