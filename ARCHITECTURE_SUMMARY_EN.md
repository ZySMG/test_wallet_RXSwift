# Trust Wallet   - Architecture Summary

## Project Overview

Trust Wallet   is an iOS-based cryptocurrency wallet application that supports storage, transfer, and management of ETH, USDC, USDT, and other tokens on the Ethereum network. The project adopts Clean Architecture patterns to ensure code maintainability, testability, and scalability.

## 1. Functional Module Architecture

### 1.1 Wallet Creation and Management Module

#### Features
- Generate new wallets (mnemonic phrases)
- Import existing wallets
- Multi-wallet management
- Wallet switching
- Account deletion

#### Development Patterns
- **MVVM + Coordinator Pattern**: Separation of business logic and UI presentation
- **Use Case Pattern**: Encapsulation of specific business operations
- **Singleton Pattern**: `WalletManagerSingleton` for unified wallet state management
- **Reactive Programming**: RxSwift for reactive data binding

#### Technical Highlights
```swift
// 1. Reactive Wallet Management
class WalletManagerSingleton {
    let currentWalletSubject = BehaviorRelay<Wallet?>(value: nil)
    let allWalletsSubject = BehaviorRelay<[Wallet]>(value: [])
}

// 2. Secure Mnemonic Storage
KeychainStorageService().store(key: "mnemonic_\(wallet.address)", value: mnemonic)

// 3. Multi-wallet State Synchronization
NotificationCenter.default.post(name: .walletSwitched, object: newWallet)
```

#### Core Files
- `GenerateMnemonicUseCase.swift` - Generate new wallets
- `ImportWalletUseCase.swift` - Import wallets
- `WalletManagerSingleton.swift` - Wallet state management
- `WalletManagementViewController.swift` - Wallet management UI

### 1.2 Asset Refresh Logic Module

#### Features
- Real-time balance queries
- Multi-currency support (ETH, USDC, USDT)
- Price information retrieval
- Caching mechanism

#### Development Patterns
- **Repository Pattern**: Abstract data access layer
- **Service Layer Pattern**: Encapsulate external API calls
- **Observer Pattern**: Balance change notifications
- **Cache Pattern**: Reduce API call frequency

#### Technical Highlights
```swift
// 1. Etherscan V2 API Integration
class EtherscanV2Service {
    func getETHBalance(address: String, chainId: Int) -> Observable<String>
    func getTokenBalance(address: String, contractAddress: String, chainId: Int, decimals: Int) -> Observable<String>
}

// 2. Reactive Balance Management
class ResolveBalancesUseCase {
    func resolveBalances(for wallet: Wallet) -> Observable<[Balance]>
}

// 3. Smart Caching Strategy
class CacheService {
    func cache<T: Codable>(_ object: T, forKey key: String, expiry: TimeInterval)
}
```

#### Core Files
- `EtherscanV2Service.swift` - Etherscan API V2 service
- `EthereumService.swift` - Ethereum network service
- `ResolveBalancesUseCase.swift` - Balance resolution use case
- `CacheService.swift` - Cache service

### 1.3 Account History Module

#### Features
- Transaction history queries
- Transaction detail display
- Transaction status tracking
- Etherscan link navigation

#### Development Patterns
- **MVVM Pattern**: Separation of UI and business logic
- **Repository Pattern**: Unified data access interface
- **Factory Pattern**: Transaction object creation
- **Strategy Pattern**: Data parsing for different networks

#### Technical Highlights
```swift
// 1. Transaction History Query
class TxService {
    func getTransactionHistory(address: String, network: Network) -> Observable<[Transaction]>
}

// 2. Transaction Direction Detection
enum TransactionDirection {
    case inbound, outbound
}

// 3. Etherscan Integration
func viewTransaction(_ txHash: String) {
    let urlString = "https://sepolia.etherscan.io/tx/\(txHash)"
    UIApplication.shared.open(URL(string: urlString)!)
}
```

#### Core Files
- `TxService.swift` - Transaction history service
- `TransactionHistoryViewModel.swift` - Transaction history view model
- `TransactionDetailViewController.swift` - Transaction detail page

### 1.4 Send Transaction Module

#### Features
- Address validation
- Amount input validation
- Gas fee estimation
- Transaction signing and broadcasting
- Progress tracking

#### Development Patterns
- **MVVM Pattern**: Separation of UI and business logic
- **Use Case Pattern**: Transaction sending use case
- **State Machine Pattern**: Transaction state management
- **Observer Pattern**: Progress update notifications

#### Technical Highlights
```swift
// 1. Gas Fee Estimation
class EstimateGasUseCase {
    func estimateGas(from: String, to: String, amount: Decimal, currency: Currency, network: Network) -> Observable<Decimal>
}

// 2. Transaction Sending Flow
class SendTransactionUseCase {
    func sendTransaction(from: Wallet, to: String, amount: Decimal, currency: Currency, gasEstimate: GasEstimate, mnemonic: String) -> Observable<String>
}

// 3. Transaction State Management
enum SendStatus {
    case sending
    case success(txHash: String)
    case failed(error: Error)
}

// 4. ETH Balance Check
private func checkETHBalanceForGasFee(completion: @escaping (Bool, Decimal?) -> Void) {
    // Check if there's enough ETH to pay gas fees
}
```

#### Core Files
- `SendTransactionUseCase.swift` - Transaction sending use case
- `EstimateGasUseCase.swift` - Gas estimation use case
- `SendViewController.swift` - Send page
- `SendProgressViewController.swift` - Send progress page

## 2. Security Assurance

### 2.1 Keychain Secure Storage

#### Implementation
```swift
class KeychainStorageService {
    func store(key: String, value: String) -> Bool
    func retrieve(key: String) -> String?
    func delete(key: String) -> Bool
}
```

#### Security Features
- **Encrypted Storage**: Hardware-level encryption using iOS Keychain
- **Access Control**: Only the app can access stored data
- **Data Isolation**: Complete isolation between different apps
- **Persistence**: Data remains after device restart

#### Stored Content
- Mnemonic phrases: `mnemonic_\(walletAddress)`
- Current wallet: `current_wallet`
- Wallet list: `wallets_list`

### 2.2 Private Key Management

#### Security Features
- **Mnemonic Generation**: Using TrustWalletCore's cryptographically secure random numbers
- **Private Key Storage**: Based on TrustWalletCore's secure storage
- **Memory Security**: Private keys are immediately cleared from memory after use
- **Hardware Security**: Support for Secure Enclave (if available)

### 2.3 Transaction Security

#### Transaction Validation
```swift
// 1. Address Format Validation
extension String {
    var isValidEthereumAddressFormat: Bool {
        // EIP-55 address format validation
    }
}

// 2. Balance Check
private func checkETHBalanceForGasFee(completion: @escaping (Bool, Decimal?) -> Void) {
    // Ensure sufficient ETH to pay gas fees
}

// 3. Gas Fee Estimation
class EstimateGasUseCase {
    // Accurate gas fee estimation to avoid transaction failures
}
```

## 3. Important Notes

### 3.1 USDT Faucet Issues

#### Problem Description
- USDT testnet faucet is unstable
- Cannot guarantee authenticity of test tokens
- Affects development and testing workflow

#### Solution
**Use Circle's USDC Test Environment**

#### Circle USDC Testnet Information
- **Faucet Address**: https://faucet.circle.com/
- **Contract Address**: `0x94a9D9AC8a22534E3FaCa9F4e7F2E2cf85d5E4C8`
- **Network**: Sepolia Testnet
- **Decimals**: 6 decimal places
- **Advantages**:
  - Stable and reliable
  - Real test tokens
  - Official support
  - Fast crediting

#### Testing Workflow
1. Visit https://faucet.circle.com/
2. Enter wallet address
3. Claim test USDC
4. Verify balance in the app
5. Perform transfer tests

### 3.2 ETH Balance Assurance

#### Importance
- **Gas Fee Payment**: All transactions require ETH to pay gas fees
- **Transaction Success**: Insufficient ETH balance causes transaction failures
- **User Experience**: Avoid user confusion and transaction failures

#### Implementation
```swift
// Pre-send ETH balance check
private func checkETHBalanceForGasFee(completion: @escaping (Bool, Decimal?) -> Void) {
    // 1. Get ETH balance
    // 2. Calculate required gas fees
    // 3. Check if balance is sufficient
    // 4. Show appropriate prompts
}

// Error messages
let errorMessage = (ethBalance ?? 0) == 0 ?
    "No ETH balance. Please deposit some ETH to pay for gas fees." :
    "Insufficient ETH balance to pay gas fees. Please deposit more ETH."
```

#### ETH Acquisition Methods
- **Sepolia Faucet**: https://sepoliafaucet.com/
- **Alchemy Faucet**: https://sepoliafaucet.com/
- **Infura Faucet**: https://www.infura.io/faucet/sepolia

#### Recommended Balance
- **Minimum Balance**: 0.01 ETH
- **Recommended Balance**: 0.05 ETH
- **Gas Fees**: Approximately 0.001-0.005 ETH per transaction

## 4. Technology Stack Summary

### 4.1 Core Frameworks
- **UIKit**: iOS native UI framework
- **RxSwift/RxCocoa**: Reactive programming
- **Alamofire**: Network requests
- **TrustWalletCore**: Encryption and wallet functionality

### 4.2 Architecture Patterns
- **Clean Architecture**: Layered architecture
- **MVVM**: View-Model pattern
- **Coordinator**: Navigation coordination
- **Repository**: Data access abstraction
- **Use Case**: Business use case encapsulation

### 4.3 Design Principles
- **Single Responsibility**: Each class has only one responsibility
- **Open/Closed Principle**: Open for extension, closed for modification
- **Dependency Inversion**: Depend on abstractions, not concrete implementations
- **Interface Segregation**: Use small, focused interfaces

## 5. Project Structure

```
Web3Wallet/
├── App/                    # Application entry and dependency injection
├── Common/                 # Common utilities and extensions
├── Data/                   # Data layer
│   ├── Ethereum/          # Ethereum-related services
│   ├── Network/           # Network services
│   └── Storage/           # Storage services
├── Domain/                # Domain layer
│   ├── Entities/          # Entity objects
│   ├── UseCases/          # Business use cases
│   └── Services/          # Domain services
└── Presentation/          # Presentation layer
    ├── Scenes/            # Specific pages
    ├── Coordinators/      # Navigation coordinators
    └── Components/        # Common components
```

## 6. Summary

Trust Wallet   adopts modern iOS development architecture, implementing a feature-complete, secure, and reliable cryptocurrency wallet application through layered design, reactive programming, and secure storage. The project particularly emphasizes user experience and security, ensuring smooth development and testing through ETH balance checks and Circle USDC test environment.

---

**Last Updated**: 2025
**Developer**: Zy
