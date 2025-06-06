# Elevenlabs Conversational AI Swift SDK

![convai222](https://github.com/user-attachments/assets/ca4fa726-5e98-4bbc-91b2-d055e957df7d)

Elevenlabs Conversational AI Swift SDK is a framework designed to integrate ElevenLabs' powerful conversational AI capabilities into your Swift applications. Leverage advanced audio processing and seamless WebSocket communication to create interactive and intelligent conversational voivce experiences.

For detailed documentation, visit the [ElevenLabs Swift SDK documentation](https://elevenlabs.io/docs/conversational-ai/libraries/conversational-ai-sdk-swift).

> [!NOTE]  
> This library is launching to primarily support Conversational AI. The support for speech synthesis and other more generic use cases is planned for the future.

## Install

Add the Elevenlabs Conversational AI Swift SDK to your project using Swift Package Manager:

1. Open Your Project in Xcode
   - Navigate to your project directory and open it in Xcode.
2. Add Package Dependency
   - Go to `File` > `Add Packages...`
3. Enter Repository URL
   - Input the following URL: `https://github.com/elevenlabs/ElevenLabsSwift`
4. Select Version
5. Import the SDK
   ```swift
   import ElevenLabsSDK
   ```
6. Ensure `NSMicrophoneUsageDescription` is added to your Info.plist to explain microphone access.

## Usage

### Setting Up a Conversation Session

1. Configure the Session
   Create a `SessionConfig` with either an `agendId` or `signedUrl`.

   ```swift
   let config = ElevenLabsSDK.SessionConfig(agentId: "your-agent-id")
   ```

2. Define Callbacks
   Implement callbacks to handle various conversation events.

   ```swift
   var callbacks = ElevenLabsSDK.Callbacks()
   callbacks.onConnect = { conversationId in
       print("Connected with ID: \(conversationId)")
   }
   callbacks.onMessage = { message, role in
       print("\(role.rawValue): \(message)")
   }
   callbacks.onError = { error, info in
       print("Error: \(error), Info: \(String(describing: info))")
   }
   callbacks.onStatusChange = { status in
       print("Status changed to: \(status.rawValue)")
   }
   callbacks.onModeChange = { mode in
       print("Mode changed to: \(mode.rawValue)")
   }
   ```

3. Start the Conversation
   Initiate the conversation session asynchronously.

   ```swift
   Task {
       do {
           let conversation = try await ElevenLabsSDK.Conversation.startSession(config: config, callbacks: callbacks)
           // Use the conversation instance as needed
       } catch {
           print("Failed to start conversation: \(error)")
       }
   }
   ```

### Advanced Configuration

1. Using Client Tools

   ```swift
   var clientTools = ElevenLabsSDK.ClientTools()
   clientTools.register("weather", handler: { async parameters throws -> String? in
       print("Weather parameters received:", parameters)
       ...
   })

   let conversation = try await ElevenLabsSDK.Conversation.startSession(
       config: config,
       callbacks: callbacks,
       clientTools: clientTools
   )
   ```

2. Using Overrides

   ```swift
   let overrides = ElevenLabsSDK.ConversationConfigOverride(
       agent: ElevenLabsSDK.AgentConfig(
           prompt: ElevenLabsSDK.AgentPrompt(prompt: "You are a helpful assistant"),
           language: .en
       )
   )

   let config = ElevenLabsSDK.SessionConfig(
       agentId: "your-agent-id",
       overrides: overrides
   )
   ```

### Manage the Session

- End Session

  ```swift
  conversation.endSession()
  ```

- Control Recording

  ```swift
  conversation.startRecording()
  conversation.stopRecording()
  ```

## Example

Explore examples in the [ElevenLabs Examples repository](https://github.com/elevenlabs/elevenlabs-examples/tree/main/examples/conversational-ai/swift).
