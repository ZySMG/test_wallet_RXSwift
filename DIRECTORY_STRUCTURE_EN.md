# Trust Wallet 2.0 - Directory Structure Summary

## 📁 Project Root Directory Structure

```
trust_wallet2/
├── 📄 Project Configuration Files
│   ├── Podfile                    # CocoaPods dependency management
│   ├── Podfile.lock              # Dependency version lock
│   ├── trust_wallet2.xcodeproj/  # Xcode project file
│   └── trust_wallet2.xcworkspace # Xcode workspace
│
├── 📚 Project Documentation
│   ├── README.md                           # Project description
│   ├── PROJECT_SUMMARY.md                  # Project functionality summary
│   ├── API_ROUTES.md                       # API routes documentation
│   ├── API_CONFIGURATION_GUIDE.md          # API configuration guide
│   ├── ARCHITECTURE_SUMMARY.md             # Architecture summary (Chinese)
│   ├── ARCHITECTURE_SUMMARY_EN.md          # Architecture summary (English)
│   ├── SWIFT_FILES_STATISTICS.md           # Swift files statistics
│   └── PROJECT_FUNCTIONALITY_MINDMAP.md    # Functionality mind map
│
└── 📱 Main Application Directory
    └── Web3Wallet/                         # Core application code
```

## 🏗️ Web3Wallet Core Architecture

### 📱 App Layer - Application Entry and Dependency Injection
```
Web3Wallet/App/
├── AppContainer.swift           # Dependency injection container
├── ApplicationCoordinator.swift # Application coordinator
└── TrustWallet2App.swift       # Application entry point
```

**Responsibilities**:
- Application startup and initialization
- Dependency injection management
- Global navigation coordination

### 🔧 Common Layer - Common Utilities and Extensions
```
Web3Wallet/Common/
├── Extensions/                  # Swift extensions
│   ├── Date+Extensions.swift    # Date extensions
│   ├── Decimal+Extensions.swift # Decimal extensions
│   └── String+Extensions.swift  # String extensions
│
├── UI/                         # Common UI components
│   └── Toast.swift             # Toast message component
│
└── Utilities/                  # Utility classes
    ├── EIP55.swift             # Ethereum address format validation
    ├── Logger.swift            # Logging utility
    └── QRCodeGenerator.swift   # QR code generator
```

**Responsibilities**:
- Provide common utilities and extensions
- Encapsulate frequently used UI components
- Implement cross-module shared functionality

### 💾 Data Layer - Data Access and Network Services
```
Web3Wallet/Data/
├── Cache/                      # Cache services
│   └── CacheService.swift      # Cache management
│
├── Ethereum/                   # Ethereum-related services
│   ├── EthereumService.swift   # Ethereum network service
│   ├── EtherscanV2Service.swift # Etherscan V2 API
│   ├── TokenService.swift      # Token service
│   └── TxService.swift         # Transaction service
│
├── Network/                    # Network configuration
│   ├── APIEndpoints.swift      # API endpoints definition
│   ├── APIKeys.swift           # API keys management
│   ├── AlternativePriceEndpoints.swift # Alternative price endpoints
│   └── NetworkService.swift    # Network service base class
│
├── Price/                      # Price services
│   ├── AlternativePriceModels.swift # Alternative price models
│   ├── MultiSourcePriceService.swift # Multi-source price service
│   └── PriceService.swift      # Price service
│
├── Storage/                    # Storage services
│   ├── KeychainStorageService.swift # Keychain storage
│   └── PreferencesStorage.swift # Preferences storage
│
└── Vault/                      # Vault services
    └── VaultService.swift      # Vault management
```

**Responsibilities**:
- Network requests and data fetching
- Local data storage
- Cache management
- External API integration

### 🎯 Domain Layer - Business Logic and Entities
```
Web3Wallet/Domain/
├── Entities/                   # Entity objects
│   ├── Balance.swift           # Balance entity
│   ├── Currency.swift          # Currency entity
│   ├── GasEstimate.swift       # Gas estimate entity
│   ├── Network.swift           # Network entity
│   ├── SendStatus.swift        # Send status entity
│   ├── Transaction.swift       # Transaction entity
│   └── Wallet.swift            # Wallet entity
│
├── Services/                   # Domain services
│   ├── DerivationService.swift # Derivation service
│   ├── SessionLock.swift       # Session lock
│   ├── WalletManager.swift     # Wallet manager
│   ├── WalletPersistenceService.swift # Wallet persistence
│   └── WalletStore.swift       # Wallet store
│
├── UseCases/                   # Business use cases
│   ├── EstimateGasUseCase.swift # Gas estimation use case
│   ├── FetchTxHistoryUseCase.swift # Transaction history use case
│   ├── GenerateMnemonicUseCase.swift # Generate mnemonic use case
│   ├── ImportWalletUseCase.swift # Import wallet use case
│   ├── ResolveBalancesUseCase.swift # Balance resolution use case
│   └── SendTransactionUseCase.swift # Send transaction use case
│
└── Validation/                 # Validation services
    ├── AddressValidator.swift  # Address validation
    └── MnemonicValidator.swift # Mnemonic validation
```

**Responsibilities**:
- Define business entities and rules
- Implement core business logic
- Provide business use case encapsulation
- Data validation and verification

### 🎨 Presentation Layer - User Interface and Interaction
```
Web3Wallet/Presentation/
├── Components/                 # Common components
│   └── TokenListView.swift     # Token list component
│
├── Coordinators/               # Navigation coordinators
│   ├── Coordinator.swift       # Coordinator base class
│   ├── OnboardingCoordinator.swift # Onboarding flow coordinator
│   └── WalletCoordinator.swift # Wallet functionality coordinator
│
└── Scenes/                     # Specific pages
    ├── CreateWallet/           # Create wallet
    │   ├── CreateWalletViewController.swift
    │   └── MnemonicViewController.swift
    │
    ├── NetworkSelection/       # Network selection
    │   └── NetworkSelectionViewController.swift
    │
    ├── Receive/                # Receive page
    │   └── ReceiveViewController.swift
    │
    ├── Send/                   # Send page
    │   ├── SendProgressViewController.swift
    │   ├── SendViewController.swift
    │   └── SendViewModel.swift
    │
    ├── Settings/               # Settings page
    │   └── SettingsViewController.swift
    │
    ├── TransactionHistory/     # Transaction history
    │   ├── TransactionDetailViewController.swift
    │   ├── TransactionHistoryViewController.swift
    │   ├── TransactionHistoryViewModel.swift
    │   └── TransactionListView.swift
    │
    ├── WalletHome/             # Wallet home
    │   ├── WalletHomeViewController.swift
    │   └── WalletHomeViewModel.swift
    │
    ├── WalletManagement/       # Wallet management
    │   ├── AddAccountViewController.swift
    │   ├── ImportWalletViewController.swift
    │   ├── WalletManagementViewController.swift
    │   └── WalletManagementViewModel.swift
    │
    └── WelcomeViewController.swift # Welcome page
```

**Responsibilities**:
- User interface presentation
- User interaction handling
- Page navigation management
- View state management

### 📦 Resources Layer - Resource Files
```
Web3Wallet/Resources/
└── Localizable.strings         # Localization strings
```

## 📊 Directory Statistics

### 📈 File Count Statistics
- **Total Directories**: 35
- **Swift Files**: 68
- **Documentation Files**: 8
- **Configuration Files**: 4

### 🏗️ Architecture Layer Statistics
```
App Layer:        3 files  (4.4%)
Common Layer:     7 files  (10.3%)
Data Layer:       15 files (22.1%)
Domain Layer:     20 files (29.4%)
Presentation Layer: 23 files (33.8%)
```

### 📱 Feature Module Statistics
```
Wallet Management:       8 files  (11.8%)
Transaction Features:    6 files  (8.8%)
Network Services:        4 files  (5.9%)
Data Storage:           3 files  (4.4%)
Common Utilities:       7 files  (10.3%)
Other Features:         40 files (58.8%)
```

## 🎯 Architecture Characteristics

### ✅ Advantages
1. **Clear Layered Architecture**: Strictly follows Clean Architecture layers
2. **Separation of Concerns**: Each layer has clear responsibility boundaries
3. **Modular Design**: Functional modules are independent and easy to maintain
4. **Scalability**: Easy to add new features and modules
5. **Testability**: Each layer can be tested independently

### 🔧 Design Principles
1. **Single Responsibility**: Each class has only one responsibility
2. **Dependency Inversion**: Depend on abstractions, not concrete implementations
3. **Open/Closed Principle**: Open for extension, closed for modification
4. **Interface Segregation**: Use small, focused interfaces

### 📋 Naming Conventions
- **File Naming**: Use PascalCase, e.g., `SendViewController.swift`
- **Directory Naming**: Use PascalCase, e.g., `WalletManagement`
- **Class Naming**: Use PascalCase, e.g., `WalletManagerSingleton`
- **Method Naming**: Use camelCase, e.g., `handleSendTransaction`

## 🚀 Extension Suggestions

### 📈 Future Extension Directions
1. **Multi-chain Support**: Add support for other blockchain networks
2. **DeFi Features**: Integrate decentralized finance protocols
3. **NFT Support**: Add NFT storage and trading functionality
4. **Social Features**: Add wallet-to-wallet transfers and social features

### 🔧 Technical Optimizations
1. **Performance Optimization**: Optimize network requests and caching strategies
2. **Security Enhancement**: Strengthen private key management and transaction security
3. **User Experience**: Optimize interface interactions and response speed
4. **Code Quality**: Improve test coverage and code quality

---

**Document Generation Time**: 2025
**Architecture Pattern**: Clean Architecture + MVVM
