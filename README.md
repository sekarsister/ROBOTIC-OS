# 📊 FLOWCHARTS - Arduino OS PKM Project
## Kumpulan Diagram Alur Sistem Lengkap (Mermaid Format)

---

## 📑 Daftar Isi
1. [Arsitektur Sistem Keseluruhan](#1-arsitektur-sistem-keseluruhan)
2. [Data Flow Architecture](#2-data-flow-architecture)
3. [Code Development Flow](#3-code-development-flow)
4. [Remote Access Flow](#4-remote-access-flow)
5. [AI Assistance Flow](#5-ai-assistance-flow)
6. [Dashboard Builder Flow](#6-dashboard-builder-flow)
7. [Developer vs End-User Mode Flow](#7-developer-vs-end-user-mode-flow)
8. [Cloudflare Tunnel Architecture](#8-cloudflare-tunnel-architecture)
9. [Sprint Development Flow](#9-sprint-development-flow)
10. [Kompilasi dan Upload Flow](#10-kompilasi-dan-upload-flow)
11. [WebSocket Communication Flow](#11-websocket-communication-flow)
12. [Backend API Architecture](#12-backend-api-architecture)
13. [Usability Testing Flow](#13-usability-testing-flow)
14. [Project Lifecycle](#14-project-lifecycle)
15. [OTA Update Flow](#15-ota-update-flow)
16. [Serial Communication Flow](#16-serial-communication-flow)

---

## 1. Arsitektur Sistem Keseluruhan

```mermaid
flowchart TB
    subgraph USER_LAYER["👤 USER LAYER"]
        WB[🌐 Web Browser]
        MA[📱 Mobile App]
        DA[🖥️ Desktop App]
    end
    
    subgraph APPLICATION_LAYER["⚙️ APPLICATION LAYER"]
        subgraph WebIDE["Web-Based Arduino IDE"]
            CE[📝 Code Editor<br/>Monaco/CodeMirror]
            AI[🤖 AI Assistant<br/>Code Gen & Debug]
            CI[🔧 Compiler<br/>Integration]
            UM[📤 Upload<br/>Manager]
        end
        
        subgraph CommPanel["Communication Panel"]
            SM[📡 Serial Monitor]
            DV[📊 Data Visualization]
            UI[🎛️ Control Interface]
        end
    end
    
    subgraph SERVICE_LAYER["🔌 SERVICE LAYER"]
        ACLI[⚡ Arduino CLI<br/>Compilation & Upload]
        CFD[☁️ Cloudflared<br/>Tunneling]
        WS[🌐 Web Server<br/>Nginx/Go]
        AIE[🧠 AI Engine<br/>Local LLM]
        DB[(🗄️ Database<br/>SQLite)]
        FM[📁 File Manager]
    end
    
    subgraph OS_LAYER["🐧 OPERATING SYSTEM LAYER"]
        LOS[Lightweight Linux<br/>Custom Debian/Yocto]
        SYS[Systemd Services]
        NET[Network Stack]
    end
    
    subgraph HARDWARE_LAYER["🔩 HARDWARE LAYER"]
        USB[🔌 USB Interface<br/>Arduino]
        NW[📶 Network<br/>WiFi/Ethernet]
        ST[💾 Storage<br/>SSD/eMMC]
    end
    
    USER_LAYER --> APPLICATION_LAYER
    APPLICATION_LAYER --> SERVICE_LAYER
    SERVICE_LAYER --> OS_LAYER
    OS_LAYER --> HARDWARE_LAYER
    
    WB --> WebIDE
    MA --> CommPanel
    DA --> CommPanel
    
    CE --> ACLI
    AI --> AIE
    CI --> ACLI
    SM --> USB
    
    CFD --> NW
    WS --> NET
    DB --> ST
```

---

## 2. Data Flow Architecture

```mermaid
flowchart LR
    subgraph CODE_DEV["1️⃣ Code Development Flow"]
        A1[👤 User Writes Code] --> A2[🤖 AI Assists]
        A2 --> A3[⚡ Compile<br/>Arduino CLI]
        A3 --> A4[📤 Upload to Board]
        A4 --> A5[📡 Serial Feedback]
        A5 --> A6[📊 Dashboard<br/>Visualization]
    end
    
    subgraph REMOTE_ACCESS["2️⃣ Remote Access Flow"]
        B1[🌐 Internet] --> B2[☁️ Cloudflare Edge]
        B2 --> B3[🔐 Encrypted Tunnel]
        B3 --> B4[🖥️ Local Web Server]
        B4 --> B5[📟 Arduino IDE/<br/>Dashboard]
        B5 --> B6[🔌 USB]
        B6 --> B7[🎛️ Arduino Board]
    end
    
    subgraph AI_ASSIST["3️⃣ AI Assistance Flow"]
        C1[📝 Code Context] --> C2[🧠 Local LLM]
        C2 --> C3[💡 Code Suggestions/<br/>Generation]
        C3 --> C4[✅ User Approval]
        C4 --> C5[📝 Integration<br/>to Editor]
    end
```

---

## 3. Code Development Flow

```mermaid
flowchart TD
    START([🚀 Start Development]) --> OPEN[📂 Open Project / New File]
    OPEN --> WRITE[📝 Write Arduino Code]
    WRITE --> NEED_HELP{Need AI Help?}
    
    NEED_HELP -->|Yes| AI_PROMPT[💬 Enter AI Prompt]
    AI_PROMPT --> AI_PROCESS[🤖 AI Processes Request]
    AI_PROCESS --> AI_SUGGEST[💡 AI Suggests Code]
    AI_SUGGEST --> ACCEPT{Accept?}
    ACCEPT -->|Yes| INSERT[📥 Insert to Editor]
    ACCEPT -->|No| AI_PROMPT
    INSERT --> WRITE
    
    NEED_HELP -->|No| SAVE[💾 Save Code]
    SAVE --> COMPILE[⚙️ Click Compile]
    COMPILE --> COMPILE_RESULT{Compilation<br/>Success?}
    
    COMPILE_RESULT -->|Error| VIEW_ERROR[🔴 View Error Log]
    VIEW_ERROR --> FIX_OPTION{Fix Option}
    FIX_OPTION -->|Manual| WRITE
    FIX_OPTION -->|AI Fix| AI_FIX[🔧 AI Auto-Fix]
    AI_FIX --> WRITE
    
    COMPILE_RESULT -->|Success| SELECT_BOARD[📟 Select Board & Port]
    SELECT_BOARD --> UPLOAD[📤 Upload to Board]
    UPLOAD --> UPLOAD_RESULT{Upload<br/>Success?}
    
    UPLOAD_RESULT -->|Error| CHECK_CONNECTION[🔌 Check Connection]
    CHECK_CONNECTION --> SELECT_BOARD
    
    UPLOAD_RESULT -->|Success| SERIAL_MONITOR[📡 Open Serial Monitor]
    SERIAL_MONITOR --> TEST[🧪 Test & Debug]
    TEST --> SATISFIED{Satisfied?}
    
    SATISFIED -->|No| WRITE
    SATISFIED -->|Yes| FINISH([✅ Development Complete])
```

---

## 4. Remote Access Flow (Cloudflare Tunnel)

```mermaid
flowchart TB
    subgraph INTERNET["🌐 INTERNET"]
        USER[👤 Remote User<br/>Any Location]
        CF_EDGE[☁️ Cloudflare Edge<br/>Global Network 300+ Cities]
    end
    
    subgraph CLOUDFLARE["☁️ CLOUDFLARE SERVICES"]
        DNS[📍 DNS Resolution]
        SSL[🔐 SSL/TLS Termination]
        WAF[🛡️ Web Application Firewall]
        DDOS[⚔️ DDoS Protection]
        ZTNA[🔒 Zero Trust Access]
    end
    
    subgraph LOCAL_NETWORK["🏠 LOCAL NETWORK (No Port Forwarding)"]
        CFD[🔌 Cloudflared Daemon]
        WEB_SERVER[🖥️ Web Server<br/>Go/Fiber :3000]
        ARDUINO_IDE[📝 Arduino IDE<br/>Web Interface]
        SERIAL_COMM[📡 Serial Communication]
        ARDUINO[🎛️ Arduino Board<br/>USB Connected]
    end
    
    USER -->|HTTPS Request| CF_EDGE
    CF_EDGE --> DNS
    DNS --> SSL
    SSL --> WAF
    WAF --> DDOS
    DDOS --> ZTNA
    
    ZTNA <-->|Encrypted Tunnel<br/>Outbound Only| CFD
    CFD --> WEB_SERVER
    WEB_SERVER --> ARDUINO_IDE
    ARDUINO_IDE --> SERIAL_COMM
    SERIAL_COMM --> ARDUINO
    
    style CLOUDFLARE fill:#f9a825,stroke:#f57f17
    style LOCAL_NETWORK fill:#4caf50,stroke:#2e7d32
```

---

## 5. AI Assistance Flow

```mermaid
flowchart TD
    START([👤 User Request]) --> REQUEST_TYPE{Request Type}
    
    REQUEST_TYPE -->|Generate Code| GEN_FLOW
    REQUEST_TYPE -->|Explain Code| EXP_FLOW
    REQUEST_TYPE -->|Fix Bug| FIX_FLOW
    
    subgraph GEN_FLOW["🔧 Code Generation"]
        G1[📝 User Prompt] --> G2[🔍 Analyze Context]
        G2 --> G3[📟 Detect Hardware]
        G3 --> G4[🧠 LLM Processing<br/>Llama 3.2]
        G4 --> G5[⚡ Generate Code]
        G5 --> G6[✅ Validate Syntax]
        G6 --> G7[📤 Return Code]
    end
    
    subgraph EXP_FLOW["📖 Code Explanation"]
        E1[📝 Select Code] --> E2[🧠 LLM Analysis]
        E2 --> E3[📊 Parse Structure]
        E3 --> E4[💬 Generate<br/>Explanation]
        E4 --> E5[📝 Add Comments]
    end
    
    subgraph FIX_FLOW["🔧 Bug Fixing"]
        F1[🔴 Error Message] --> F2[📝 Code Context]
        F2 --> F3[🔍 Identify Issue]
        F3 --> F4[🧠 LLM Suggest Fix]
        F4 --> F5[✏️ Apply Fix]
        F5 --> F6[✅ Verify Solution]
    end
    
    GEN_FLOW --> DISPLAY[📋 Display Result]
    EXP_FLOW --> DISPLAY
    FIX_FLOW --> DISPLAY
    
    DISPLAY --> USER_ACTION{User Action}
    USER_ACTION -->|Accept| APPLY[✅ Apply to Editor]
    USER_ACTION -->|Modify| REGENERATE[🔄 Regenerate]
    USER_ACTION -->|Reject| END([❌ Cancel])
    
    REGENERATE --> REQUEST_TYPE
    APPLY --> END2([✅ Complete])
```

---

## 6. Dashboard Builder Flow

```mermaid
flowchart TD
    subgraph DEVELOPER_SIDE["👨‍💻 DEVELOPER SIDE"]
        D1([Start]) --> D2[📝 Write Arduino Code]
        D2 --> D3[📤 Upload to Board]
        D3 --> D4[✅ Code Running]
        
        D4 --> D5[🎨 Open Dashboard Builder]
        D5 --> D6[➕ Create New Dashboard]
        D6 --> D7{Choose Start}
        D7 -->|Template| D8[📋 Select Template]
        D7 -->|Blank| D9[📄 Start Blank]
        
        D8 --> D10
        D9 --> D10[🔧 Drag-Drop Widgets]
        
        D10 --> WIDGETS
        subgraph WIDGETS["Widget Configuration"]
            W1[📊 Gauge - Temperature]
            W2[📈 Chart - History]
            W3[🔘 Button - Control]
            W4[📏 Slider - Adjust]
        end
        
        WIDGETS --> D11[🔗 Bind to Serial Data]
        D11 --> D12[🎨 Customize Styling]
        D12 --> D13[🧪 Test Dashboard]
        D13 --> D14[📤 PUBLISH]
        D14 --> D15[🔗 Get Shareable URL]
        D15 --> D16[📨 Share to End-User]
    end
    
    subgraph ENDUSER_SIDE["👨‍🌾 END-USER SIDE"]
        E1([Receive URL]) --> E2[🌐 Open URL in Browser]
        E2 --> E3{Protected?}
        E3 -->|Yes| E4[🔐 Enter Password]
        E3 -->|No| E5
        E4 --> E5[📊 View Dashboard]
        
        E5 --> E6["👁️ Monitoring<br/>- View Gauges<br/>- See Charts<br/>- Check Status"]
        E5 --> E7["🎮 Control<br/>- Click Buttons<br/>- Adjust Sliders<br/>- Toggle Switches"]
        
        E6 --> E8[🔄 Auto Refresh]
        E7 --> E9[📡 Send Command<br/>to Arduino]
        
        E9 --> E10[⚡ Arduino Executes]
    end
    
    D16 -.->|Share URL| E1
```

---

## 7. Developer vs End-User Mode Flow

```mermaid
flowchart LR
    subgraph DEVELOPER_MODE["🔧 DEVELOPER MODE"]
        direction TB
        D_ACCESS[Full System Access]
        D_IDE[📝 Code Editor & IDE]
        D_BUILD[🏗️ Dashboard Builder]
        D_DEBUG[🔍 Debug & Test Tools]
        D_CONFIG[⚙️ Configuration]
        D_TERM[💻 Terminal Access]
        
        D_ACCESS --> D_IDE
        D_ACCESS --> D_BUILD
        D_ACCESS --> D_DEBUG
        D_ACCESS --> D_CONFIG
        D_ACCESS --> D_TERM
    end
    
    subgraph ENDUSER_MODE["👤 END-USER MODE"]
        direction TB
        E_ACCESS[Dashboard Only Access]
        E_MONITOR["📊 Monitor<br/>- Gauges<br/>- Charts<br/>- Status"]
        E_CONTROL["🎮 Control<br/>- Buttons<br/>- Sliders"]
        E_NO["❌ NO ACCESS<br/>- No IDE<br/>- No Code<br/>- No Settings"]
        
        E_ACCESS --> E_MONITOR
        E_ACCESS --> E_CONTROL
        E_ACCESS --> E_NO
    end
    
    subgraph COMPARISON["📋 COMPARISON"]
        direction TB
        C1["Interface: IDE + Builder<br/>vs Control Panel Only"]
        C2["Access: Full System<br/>vs Published Dashboard"]
        C3["URL: /ide, /builder<br/>vs /dashboard/xyz"]
        C4["Auth: Owner Credentials<br/>vs Optional Password"]
        C5["Learning: Programming Required<br/>vs Zero Programming"]
    end
    
    DEVELOPER_MODE --> COMPARISON
    ENDUSER_MODE --> COMPARISON
    
    style DEVELOPER_MODE fill:#1565c0,color:#fff
    style ENDUSER_MODE fill:#2e7d32,color:#fff
```

---

## 8. Cloudflare Tunnel Setup Flow

```mermaid
flowchart TD
    START([🚀 First Boot]) --> DETECT[🔍 Detect Network<br/>Connection]
    DETECT --> NETWORK{Network<br/>Available?}
    
    NETWORK -->|No| WAIT[⏳ Wait & Retry]
    WAIT --> DETECT
    
    NETWORK -->|Yes| INSTALL[📦 Install<br/>Cloudflared Daemon]
    INSTALL --> AUTH[🔐 Authenticate<br/>via Browser OAuth]
    
    AUTH --> AUTH_RESULT{Auth<br/>Success?}
    AUTH_RESULT -->|No| MANUAL[📝 Show Manual<br/>Setup Instructions]
    MANUAL --> END_FAIL([❌ Manual Setup Required])
    
    AUTH_RESULT -->|Yes| CREATE[🔧 Create Tunnel<br/>with Unique ID]
    CREATE --> DNS[📍 Configure DNS<br/>Custom Domain or<br/>*.trycloudflare.com]
    DNS --> SERVICE[⚙️ Start Systemd<br/>Service]
    SERVICE --> VERIFY[✅ Verify Tunnel<br/>Connection]
    
    VERIFY --> VERIFY_RESULT{Tunnel<br/>Active?}
    VERIFY_RESULT -->|No| RETRY[🔄 Retry Connection]
    RETRY --> SERVICE
    
    VERIFY_RESULT -->|Yes| DISPLAY[📋 Display<br/>Public URL]
    DISPLAY --> ENABLE_BOOT[🔄 Enable Auto-Start<br/>on Boot]
    ENABLE_BOOT --> END_SUCCESS([✅ Setup Complete!<br/>Access: https://your-domain.com])
    
    style START fill:#4caf50,color:#fff
    style END_SUCCESS fill:#4caf50,color:#fff
    style END_FAIL fill:#f44336,color:#fff
```

---

## 9. Sprint Development Flow (Agile)

```mermaid
gantt
    title 📅 Sprint Timeline - Arduino OS Development
    dateFormat  YYYY-MM-DD
    axisFormat  %b %d
    
    section Foundation
    Sprint 1-2: Core OS & Base System     :s1, 2026-01-01, 28d
    
    section IDE Development
    Sprint 3-4: Web IDE Development       :s2, after s1, 28d
    
    section AI Integration
    Sprint 5-6: AI Integration & Code Gen :s3, after s2, 28d
    
    section Tunneling
    Sprint 7-8: Cloudflare Tunnel         :s4, after s3, 28d
    
    section Testing
    Sprint 9-10: Testing & Optimization   :s5, after s4, 28d
```

```mermaid
flowchart TB
    subgraph SPRINT_1_2["🔧 SPRINT 1-2: Foundation"]
        S1[Setup Yocto/Debian<br/>Base Image]
        S2[Configure Automated<br/>Installer]
        S3[Network Stack<br/>Optimization]
        S4[First Boot<br/>Wizard]
        S1 --> S2 --> S3 --> S4
    end
    
    subgraph SPRINT_3_4["💻 SPRINT 3-4: IDE Core"]
        S5[Web IDE Frontend]
        S6[Monaco Editor<br/>Integration]
        S7[Arduino CLI<br/>Backend]
        S8[Compile & Upload<br/>Mechanism]
        S5 --> S6 --> S7 --> S8
    end
    
    subgraph SPRINT_5_6["🤖 SPRINT 5-6: AI"]
        S9[Local LLM Setup<br/>Llama 3.2]
        S10[Fine-tune with<br/>Arduino Dataset]
        S11[Inference<br/>Optimization]
        S12[IDE AI<br/>Integration]
        S9 --> S10 --> S11 --> S12
    end
    
    subgraph SPRINT_7_8["☁️ SPRINT 7-8: Cloudflare"]
        S13[Cloudflared<br/>Automation]
        S14[DNS Configuration]
        S15[Security Policies]
        S16[Documentation]
        S13 --> S14 --> S15 --> S16
    end
    
    subgraph SPRINT_9_10["✅ SPRINT 9-10: Polish"]
        S17[UI/UX Improvements]
        S18[Performance<br/>Optimization]
        S19[Comprehensive<br/>Testing]
        S20[Documentation<br/>Complete]
        S17 --> S18 --> S19 --> S20
    end
    
    SPRINT_1_2 --> SPRINT_3_4 --> SPRINT_5_6 --> SPRINT_7_8 --> SPRINT_9_10
```

---

## 10. Kompilasi dan Upload Flow

```mermaid
flowchart TD
    START([📝 Click Compile]) --> READ[📖 Read Source Code<br/>from Editor]
    READ --> VALIDATE[✅ Validate<br/>Syntax]
    
    VALIDATE --> VALID{Valid?}
    VALID -->|No| ERROR1[🔴 Show Syntax<br/>Error]
    ERROR1 --> END_ERR([❌ Fix Required])
    
    VALID -->|Yes| DETECT_BOARD[📟 Detect<br/>Arduino Board]
    DETECT_BOARD --> BOARD{Board<br/>Detected?}
    
    BOARD -->|No| ERROR2[🔴 No Board Found]
    ERROR2 --> END_ERR
    
    BOARD -->|Yes| CALL_CLI[⚡ Call Arduino CLI<br/>arduino-cli compile]
    CALL_CLI --> COMPILE_RESULT{Compile<br/>Result}
    
    COMPILE_RESULT -->|Error| PARSE_ERR[📋 Parse Error<br/>Message]
    PARSE_ERR --> HIGHLIGHT[🔍 Highlight<br/>Error Line]
    HIGHLIGHT --> OFFER_FIX[🤖 Offer AI Fix?]
    OFFER_FIX --> END_ERR
    
    COMPILE_RESULT -->|Success| GEN_HEX[📦 Generate<br/>HEX File]
    GEN_HEX --> UPLOAD_PROMPT{Auto<br/>Upload?}
    
    UPLOAD_PROMPT -->|No| COMPILE_SUCCESS([✅ Compile Success<br/>Ready to Upload])
    
    UPLOAD_PROMPT -->|Yes| SELECT_PORT[🔌 Select Serial<br/>Port]
    SELECT_PORT --> UPLOAD[📤 Upload via<br/>Arduino CLI]
    UPLOAD --> UPLOAD_RESULT{Upload<br/>Result}
    
    UPLOAD_RESULT -->|Error| ERROR3[🔴 Upload Failed<br/>Check Connection]
    ERROR3 --> END_ERR
    
    UPLOAD_RESULT -->|Success| RESET[🔄 Reset Board]
    RESET --> VERIFY[✅ Verify Program<br/>Running]
    VERIFY --> END_SUCCESS([✅ Upload Complete!])
    
    style START fill:#2196f3,color:#fff
    style END_SUCCESS fill:#4caf50,color:#fff
    style END_ERR fill:#f44336,color:#fff
```

---

## 11. WebSocket Communication Flow

```mermaid
sequenceDiagram
    participant Browser as 🌐 Browser
    participant WebSocket as 🔌 WebSocket Hub
    participant Serial as 📡 Serial Manager
    participant Arduino as 🎛️ Arduino Board
    
    Note over Browser,Arduino: Connection Establishment
    Browser->>WebSocket: Connect to /ws/serial
    WebSocket->>Browser: Connection Accepted
    
    Note over Browser,Arduino: Serial Port Connection
    Browser->>WebSocket: {type: "connect", port: "COM3"}
    WebSocket->>Serial: OpenPort("COM3", 9600)
    Serial->>Arduino: Open Serial Connection
    Arduino-->>Serial: Connection OK
    Serial-->>WebSocket: Port Opened
    WebSocket-->>Browser: {status: "connected"}
    
    Note over Browser,Arduino: Bidirectional Communication
    loop Real-time Data
        Arduino->>Serial: Send Sensor Data
        Serial->>WebSocket: Forward Data
        WebSocket->>Browser: {type: "data", value: "25.5"}
        Browser->>Browser: Update UI
    end
    
    Note over Browser,Arduino: Send Command
    Browser->>WebSocket: {type: "command", cmd: "LED:ON"}
    WebSocket->>Serial: Write Command
    Serial->>Arduino: LED:ON
    Arduino-->>Serial: ACK
    Serial-->>WebSocket: Command Executed
    WebSocket-->>Browser: {status: "ok"}
    
    Note over Browser,Arduino: Disconnection
    Browser->>WebSocket: {type: "disconnect"}
    WebSocket->>Serial: ClosePort
    Serial->>Arduino: Close Connection
```

---

## 12. Backend API Architecture

```mermaid
flowchart TB
    subgraph CLIENT["🌐 CLIENT"]
        BROWSER[Web Browser]
    end
    
    subgraph SERVER["🖥️ GO FIBER SERVER :3000"]
        MIDDLEWARE["⚙️ Middleware<br/>- CORS<br/>- Logger<br/>- Recover"]
        
        subgraph API_ROUTES["/api Routes"]
            subgraph SERIAL["/ports - Serial"]
                S1[GET /ports]
                S2[POST /ports/connect]
                S3[POST /ports/disconnect]
                S4[POST /ports/send]
            end
            
            subgraph PROJECT["/projects - Management"]
                P1[GET /projects]
                P2[POST /projects]
                P3[GET /projects/:id]
                P4[PUT /projects/:id]
                P5[DELETE /projects/:id]
            end
            
            subgraph FILES["/files - File Ops"]
                F1[GET /files]
                F2[POST /files]
                F3[GET /files/:name]
                F4[DELETE /files/:name]
            end
            
            subgraph COMPILE["/compile & /upload"]
                C1[POST /compile]
                C2[POST /upload]
            end
            
            subgraph HARDWARE["/hardware"]
                H1[GET /hardware/detect]
                H2[GET /hardware/boards]
            end
            
            subgraph AI["/ai - Assistant"]
                A1[POST /ai/generate]
                A2[POST /ai/explain]
                A3[POST /ai/fix]
            end
            
            subgraph SYSTEM["/system"]
                SY1[GET /system/info]
                SY2[GET /system/status]
            end
        end
        
        WS_ROUTE["/ws/serial<br/>WebSocket"]
        STATIC["/static<br/>Static Files"]
        HEALTH["/health<br/>Health Check"]
    end
    
    subgraph INTERNAL["🔧 INTERNAL PACKAGES"]
        HANDLERS[handlers/handlers.go]
        SERIAL_MGR[serial/manager.go]
        WS_HUB[websocket/hub.go]
    end
    
    BROWSER --> MIDDLEWARE
    MIDDLEWARE --> API_ROUTES
    MIDDLEWARE --> WS_ROUTE
    MIDDLEWARE --> STATIC
    MIDDLEWARE --> HEALTH
    
    API_ROUTES --> HANDLERS
    WS_ROUTE --> WS_HUB
    HANDLERS --> SERIAL_MGR
    WS_HUB --> SERIAL_MGR
```

---

## 13. Usability Testing Flow

```mermaid
flowchart TD
    START([📋 Usability Testing]) --> RECRUIT[👥 Recruit 20-30<br/>Participants]
    
    RECRUIT --> CATEGORIZE[📊 Categorize Users]
    CATEGORIZE --> CAT1[🆕 Beginners: 10<br/>Never coded Arduino]
    CATEGORIZE --> CAT2[📈 Intermediate: 10-15<br/>2-5 Projects]
    CATEGORIZE --> CAT3[🎯 Advanced: 5<br/>Experienced Devs]
    
    CAT1 --> TASKS
    CAT2 --> TASKS
    CAT3 --> TASKS
    
    subgraph TASKS["📝 Testing Tasks"]
        T1[1. Install OS from USB]
        T2[2. Setup First Connection]
        T3[3. Write, Compile, Upload 'Blink']
        T4[4. Use AI to Generate<br/>Sensor Reading Code]
        T5[5. Configure Custom<br/>Dashboard]
        T6[6. Setup Remote Access<br/>via Cloudflare]
        
        T1 --> T2 --> T3 --> T4 --> T5 --> T6
    end
    
    TASKS --> COLLECT
    
    subgraph COLLECT["📊 Data Collection"]
        D1[⏱️ Task Completion Time]
        D2[❌ Error Rate]
        D3[🙋 Assistance Requests]
        D4[⭐ Satisfaction Rating<br/>1-5 Scale]
        D5[💬 Qualitative Feedback]
    end
    
    COLLECT --> SUS[📋 SUS Questionnaire<br/>10 Standard Questions]
    SUS --> CALCULATE[📈 Calculate SUS Score<br/>0-100 Scale]
    CALCULATE --> TARGET{Score > 75?}
    
    TARGET -->|Yes| SUCCESS([✅ Good Usability<br/>Target Met!])
    TARGET -->|No| IMPROVE[🔄 Identify<br/>Improvements]
    IMPROVE --> ITERATE[🔧 Iterate Design]
    ITERATE --> RETEST[↩️ Retest]
    
    style SUCCESS fill:#4caf50,color:#fff
```

---

## 14. Project Lifecycle (Design Science Research)

```mermaid
flowchart LR
    subgraph PHASE1["1️⃣ Problem Identification"]
        P1A[❌ Complex Setup<br/>Environment]
        P1B[❌ Difficult Remote<br/>Access]
        P1C[❌ Resource<br/>Limitations]
        P1D[❌ Lack of<br/>Integration]
    end
    
    subgraph PHASE2["2️⃣ Solution Objectives"]
        P2A[✅ All-in-One OS<br/>Ready-to-Use]
        P2B[✅ Setup < 15 min<br/>Automated]
        P2C[✅ Resource-Efficient<br/>1GB RAM, 4GB Storage]
        P2D[✅ Free & Open<br/>No Paid Services]
        P2E[✅ Enterprise Security<br/>Cloudflare Tunnel]
    end
    
    subgraph PHASE3["3️⃣ Design & Development"]
        P3A[🔧 Sprint 1-2:<br/>Core OS]
        P3B[💻 Sprint 3-4:<br/>Web IDE]
        P3C[🤖 Sprint 5-6:<br/>AI Integration]
        P3D[☁️ Sprint 7-8:<br/>Tunnel Setup]
        P3E[✅ Sprint 9-10:<br/>Testing]
    end
    
    subgraph PHASE4["4️⃣ Demonstration"]
        P4A[🎯 Proof of Concept]
        P4B[📋 Use Case<br/>Scenarios]
        P4C[📊 Performance<br/>Benchmarks]
    end
    
    subgraph PHASE5["5️⃣ Evaluation"]
        P5A[📋 SUS Testing]
        P5B[⏱️ Performance<br/>Metrics]
        P5C[📊 Comparative<br/>Study]
        P5D[⭐ User Survey]
    end
    
    subgraph PHASE6["6️⃣ Communication"]
        P6A[📖 Technical Docs]
        P6B[📚 User Manual]
        P6C[🌐 Open Source<br/>Repository]
        P6D[📄 Academic<br/>Publication]
    end
    
    PHASE1 --> PHASE2 --> PHASE3 --> PHASE4 --> PHASE5 --> PHASE6
```

---

## 15. OTA Update Flow

```mermaid
flowchart TD
    START([🔄 OTA Update<br/>Initiated]) --> CHECK[🔍 Check for<br/>Updates]
    CHECK --> UPDATE{Update<br/>Available?}
    
    UPDATE -->|No| END_CURRENT([✅ Already<br/>Up-to-Date])
    
    UPDATE -->|Yes| FETCH[📥 Fetch Update<br/>Metadata]
    FETCH --> VERIFY_SIG[🔐 Verify<br/>Cryptographic Signature]
    
    VERIFY_SIG --> SIG_VALID{Signature<br/>Valid?}
    SIG_VALID -->|No| ABORT[❌ Abort Update<br/>Security Risk]
    ABORT --> END_FAIL([🔴 Update Failed])
    
    SIG_VALID -->|Yes| DOWNLOAD[📦 Download<br/>Firmware Package]
    DOWNLOAD --> DELTA{Delta<br/>Update?}
    
    DELTA -->|Yes| APPLY_DELTA[📝 Apply Delta<br/>Patches Only]
    DELTA -->|No| FULL_UPDATE[📦 Full Firmware<br/>Download]
    
    APPLY_DELTA --> VERIFY_INT
    FULL_UPDATE --> VERIFY_INT[🔍 Verify Integrity<br/>Hash Check]
    
    VERIFY_INT --> INT_VALID{Integrity<br/>OK?}
    INT_VALID -->|No| ROLLBACK[↩️ Rollback to<br/>Previous Version]
    ROLLBACK --> END_FAIL
    
    INT_VALID -->|Yes| BACKUP[💾 Backup<br/>Current Version]
    BACKUP --> INSTALL[⚙️ Install<br/>New Firmware]
    INSTALL --> RESTART[🔄 Restart<br/>Services]
    RESTART --> HEALTH[🏥 Health<br/>Check]
    
    HEALTH --> HEALTHY{System<br/>Healthy?}
    HEALTHY -->|No| ROLLBACK
    HEALTHY -->|Yes| CLEANUP[🧹 Cleanup<br/>Old Files]
    CLEANUP --> END_SUCCESS([✅ Update<br/>Complete!])
    
    style START fill:#2196f3,color:#fff
    style END_SUCCESS fill:#4caf50,color:#fff
    style END_FAIL fill:#f44336,color:#fff
```

---

## 16. Serial Communication Flow

```mermaid
flowchart TD
    subgraph FRONTEND["🌐 FRONTEND (Browser)"]
        UI_SERIAL[Serial Monitor UI]
        UI_SEND[Send Message Input]
        UI_DISPLAY[Data Display]
    end
    
    subgraph WEBSOCKET["🔌 WEBSOCKET LAYER"]
        WS_CONN[WebSocket Connection<br/>/ws/serial]
        WS_HUB[WebSocket Hub<br/>Message Router]
    end
    
    subgraph BACKEND["🔧 BACKEND (Go)"]
        SERIAL_MGR[Serial Manager]
        PORT_HANDLER[Port Handler]
        BUFFER[Read/Write Buffer]
    end
    
    subgraph HARDWARE["🔩 HARDWARE"]
        USB[USB Serial Port]
        ARDUINO[Arduino Board]
    end
    
    UI_SERIAL -->|Connect| WS_CONN
    WS_CONN <-->|Bidirectional| WS_HUB
    WS_HUB --> SERIAL_MGR
    
    UI_SEND -->|Send Command| WS_CONN
    WS_HUB -->|Write| PORT_HANDLER
    PORT_HANDLER --> BUFFER
    BUFFER --> USB
    USB --> ARDUINO
    
    ARDUINO -->|Response| USB
    USB --> BUFFER
    BUFFER -->|Read| PORT_HANDLER
    PORT_HANDLER --> WS_HUB
    WS_HUB --> WS_CONN
    WS_CONN -->|Display| UI_DISPLAY
    
    style FRONTEND fill:#e3f2fd
    style WEBSOCKET fill:#fff3e0
    style BACKEND fill:#e8f5e9
    style HARDWARE fill:#fce4ec
```

---

## 📊 Comparison Flowchart: Our System vs Traditional

```mermaid
flowchart LR
    subgraph TRADITIONAL["⏱️ Traditional Setup (2-4 Hours)"]
        T1[Install OS] --> T2[Download Arduino IDE]
        T2 --> T3[Install IDE]
        T3 --> T4[Install Drivers]
        T4 --> T5[Configure Network]
        T5 --> T6[Setup Port Forwarding]
        T6 --> T7[Configure Ngrok<br/>$$$ Paid]
        T7 --> T8[Test Connection]
        T8 --> T9[✅ Ready]
    end
    
    subgraph OUR_SYSTEM["⚡ Arduino OS (< 15 Minutes)"]
        O1[Boot from USB] --> O2[First-Boot Wizard]
        O2 --> O3[Auto-Configure]
        O3 --> O4[✅ Ready!<br/>Free Cloudflare Tunnel]
    end
    
    style TRADITIONAL fill:#ffebee
    style OUR_SYSTEM fill:#e8f5e9
```

---

## 🎯 Smart Agriculture Use Case Flow

```mermaid
flowchart TD
    subgraph SETUP["🔧 Initial Setup"]
        S1[Developer Programs<br/>Arduino with Sensors]
        S2[Upload Code for:<br/>- Soil Moisture<br/>- Temperature<br/>- Water Pump]
        S3[Create Dashboard<br/>with Builder]
        S4[Publish & Share URL<br/>to Farmer]
        S1 --> S2 --> S3 --> S4
    end
    
    subgraph FARMER_USE["🌱 Daily Farmer Use"]
        F1[Farmer Opens URL<br/>on Phone/Tablet]
        F2[Views Real-Time:<br/>🌡️ Temperature<br/>💧 Moisture<br/>📊 Charts]
        F3[Controls:<br/>💧 Water Pump<br/>🌬️ Fan]
        F4[Receives Alerts:<br/>⚠️ Low Moisture<br/>🌡️ High Temp]
        F1 --> F2
        F1 --> F3
        F1 --> F4
    end
    
    subgraph AUTOMATION["⚡ Behind the Scenes"]
        A1[Arduino Reads<br/>Sensors Every Second]
        A2[Sends Data via<br/>Serial/JSON]
        A3[WebSocket Updates<br/>Dashboard Real-Time]
        A4[Farmer Command<br/>Sent to Arduino]
        A5[Pump/Fan<br/>Activated]
        A1 --> A2 --> A3
        F3 --> A4 --> A5
    end
    
    S4 -.-> F1
    
    style SETUP fill:#e3f2fd
    style FARMER_USE fill:#c8e6c9
    style AUTOMATION fill:#fff3e0
```

---

## 📈 Performance Metrics Comparison

```mermaid
pie showData
    title Resource Usage Comparison (MB RAM)
    "Arduino OS (Ours)" : 50
    "Traditional Arduino IDE" : 300
    "PlatformIO" : 200
```

```mermaid
xychart-beta
    title "Setup Time Comparison (Minutes)"
    x-axis ["Traditional", "PlatformIO", "Arduino OS (Ours)"]
    y-axis "Minutes" 0 --> 250
    bar [180, 120, 15]
```

---

## ✅ Milestone Checklist Flowchart

```mermaid
flowchart LR
    M1([🏁 M1<br/>Month 2<br/>Infrastructure]) --> M2([🏗️ M2<br/>Month 4<br/>Core System])
    M2 --> M3([🔧 M3<br/>Month 6<br/>Integration])
    M3 --> M4([🧪 M4<br/>Month 8<br/>Beta Release])
    M4 --> M5([✅ M5<br/>Month 10<br/>Final])
    
    M1 --- C1["✓ Dev Environment<br/>✓ Dataset Ready<br/>✓ Base OS Bootable"]
    M2 --- C2["✓ IDE Compiles<br/>✓ Upload Works<br/>✓ Basic Features"]
    M3 --- C3["✓ AI Integrated<br/>✓ Tunnel Working<br/>✓ Alpha Version"]
    M4 --- C4["✓ Testing Done<br/>✓ Bugs Fixed<br/>✓ Optimized"]
    M5 --- C5["✓ Report Complete<br/>✓ Documentation<br/>✓ Ready Deploy"]
    
    style M1 fill:#4caf50,color:#fff
    style M2 fill:#4caf50,color:#fff
    style M3 fill:#4caf50,color:#fff
    style M4 fill:#ff9800,color:#fff
    style M5 fill:#2196f3,color:#fff
```

---

*📅 Generated: January 2026*
*📁 Project: PKM - Arduino OS for IoT Smart System Development*
*🔧 Format: Mermaid Diagrams*
