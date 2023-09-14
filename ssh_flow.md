graph LR
    A[Client] -->|1. Client-Server Connection 
Establishment| B[Server]
    B -->|2. Server Identification| A
    A -->|3. Client Identification| B
    B -->|4. Key Exchange Initiation| A
    B -->|5. Host Key Verification| A
    A -->|6. Key Exchange| B
    A -->|7. User Authentication| B
    B -->|7. User Authentication| A
    A -->|8. Secure Session Establishment| B
    A -->|9. Interactive or Non-Interactive Session| 
B
    B -->|9. Interactive or Non-Interactive Session| 
A
    A -->|10. Session Termination| B
    B -->|10. Session Termination| A
