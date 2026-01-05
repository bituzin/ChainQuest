# 🚀 ChainQuest - Decentralized Smart Contract Learning Platform

> Learn Solidity through hands-on challenges, compete with developers worldwide, and earn XP tokens & NFT badges for your achievements.

**🌐 Built on Base L2** - Fast, cheap, and Ethereum-compatible blockchain by Coinbase

## 🌐 Dlaczego Base?

**ChainQuest działa na Base** - Layer 2 blockchain zbudowany na Optimism Stack przez Coinbase.

**Korzyści Base:**
- ⚡ **Szybkie transakcje** - 2s block time (vs 12s Ethereum)
- 💰 **Niskie fees** - ~$0.01 per transaction (vs $5-50 na Ethereum)
- 🔗 **Full EVM compatibility** - Wszystkie narzędzia Ethereum działają
- 🚀 **Rosnący ekosystem** - Setki projektów Web3
- 🏦 **Backed by Coinbase** - Infrastruktura enterprise-grade
- 🌉 **Łatwy bridge** - Szybkie transfery z/do Ethereum

**Network Info:**
- **Base Sepolia Testnet** (dla testów):
  - RPC: `https://sepolia.base.org`
  - Chain ID: `84532`
  - Explorer: https://sepolia.basescan.org
  - Faucet: https://www.coinbase.com/faucets/base-ethereum-sepolia-faucet

- **Base Mainnet** (produkcja):
  - RPC: `https://mainnet.base.org`
  - Chain ID: `8453`
  - Explorer: https://basescan.org
  - Bridge: https://bridge.base.org

![ChainQuest Banner](./docs/banner.png)

## 📋 Spis treści

- [Opis projektu](#-opis-projektu)
- [Architektura](#-architektura)
- [Smart Contracts](#-smart-contracts)
- [Frontend](#-frontend)
- [Instalacja](#-instalacja)
- [Deployment](#-deployment)
- [Użytkowanie](#-użytkowanie)
- [Rozwój](#-rozwój)

## 🎯 Opis projektu

ChainQuest to platforma edukacyjna Web3, która umożliwia:

- **Naukę przez praktykę**: Ponad 100 wyzwań programistycznych od poziomu beginner do expert
- **Weryfikację on-chain**: Wszystkie rozwiązania są walidowane za pomocą smart kontraktów
- **System nagród**: Zdobywaj XP tokeny (ERC20) i NFT badges (ERC1155) za ukończone wyzwania
- **Profil jako NFT**: Twój profil to evolving NFT (ERC721) który rośnie wraz z Tobą
- **Rankingi i konkursy**: Rywalizuj z innymi developerami o nagrody
- **Pełna decentralizacja**: Wszystkie dane i logika on-chain

## 🏗️ Architektura

### Koncepcja

```
┌─────────────────────────────────────────────────────────────┐
│                     CHAINQUEST PLATFORM                      │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  User  →  [Profile NFT]  →  [Wybór Challenge]               │
│                                  ↓                            │
│           [Code Editor]  →  [Submit Solution]                │
│                                  ↓                            │
│           [Validator]  →  [Testy On-Chain/Oracle]           │
│                                  ↓                            │
│           [Reward Distributor]  →  [XP + Badges]            │
│                                  ↓                            │
│           [Leaderboard]  →  [Ranking Global]                 │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Smart Contracts (8 Core)

```
┌──────────────────────────────────────────────────┐
│                 SMART CONTRACTS                   │
├──────────────────────────────────────────────────┤
│                                                   │
│  1. ChallengeRegistry    - Baza wyzwań           │
│  2. UserProfile (ERC721) - Profile jako NFT      │
│  3. SolutionSubmission   - Zarządzanie rozwiązaniami │
│  4. SolutionValidator    - Walidacja on-chain    │
│  5. RewardDistributor    - Dystrybucja nagród    │
│  6. ExperienceToken      - XP (ERC20)            │
│  7. AchievementBadges    - Badges (ERC1155)      │
│  8. LeaderboardManager   - Rankingi              │
│                                                   │
└──────────────────────────────────────────────────┘
```

## 📝 Smart Contracts

### 1. ChallengeRegistry

**Cel**: Centralna baza danych wszystkich wyzwań

**Funkcje kluczowe**:
- `createChallenge()` - Dodawanie nowych wyzwań
- `getChallenge()` - Pobieranie szczegółów wyzwania
- `getChallengesByDifficulty()` - Filtrowanie po trudności
- `getChallengesByCategory()` - Filtrowanie po kategorii

**Kategorie**:
- DeFi, NFT, Security, Governance, GameFi, Infrastructure, Advanced Patterns, Gas Optimization

**Poziomy trudności**:
- Beginner, Easy, Medium, Hard, Expert

### 2. UserProfile (ERC721)

**Cel**: Profile użytkowników jako ewoluujące NFT

**Dane profilu**:
```solidity
struct Profile {
    uint256 level;
    uint256 totalXP;
    uint256[] completedChallenges;
    uint256[] badges;
    uint256 createdAt;
    uint256 lastActive;
    uint256 streak;
    uint256 totalSolutions;
    uint256 bestRank;
}
```

**Leveling System**:
- Level 1: 0 XP
- Level 2: 100 XP
- Level 3: 250 XP
- ...
- Level 10: 32,000 XP

### 3. SolutionSubmission

**Cel**: Zarządzanie submissionami rozwiązań

**Flow**:
1. User submittuje rozwiązanie (adres kontraktu + IPFS hash kodu)
2. System zapisuje submission
3. Wywołuje SolutionValidator
4. Zapisuje wyniki

### 4. SolutionValidator

**Cel**: Walidacja rozwiązań

**Dwa tryby**:
- **On-chain**: Proste testy wykonywane bezpośrednio w kontrakcie
- **Oracle pattern**: Złożone testy off-chain, wyniki submitowane on-chain

**Scoring**:
- Base score (0-100) z testów
- Gas efficiency bonus
- Security score bonus
- Speed bonus

### 5. RewardDistributor

**Cel**: Dystrybucja nagród za ukończone wyzwania

**Reward Formula**:
```
XP = basePoints × difficultyMultiplier × (score/100) + bonuses
```

**Bonuses**:
- Speed bonus (szybkie rozwiązanie)
- Perfect score bonus (100% testów)
- Gas optimization bonus

**Auto-badge awarding**:
- First Blood (pierwsze wyzwanie)
- Speed Demon (szybkie rozwiązanie)
- Perfect Score (100%)
- Streak badges (7, 30 dni)

### 6. ExperienceToken (ERC20)

**Cel**: Token XP platformy

**Właściwości**:
- Non-transferable (domyślnie)
- Mintowany tylko przez RewardDistributor
- Staking z APY rewards
- Używany do unlock premium challenges

### 7. AchievementBadges (ERC1155)

**Cel**: Kolekcjonowalne odznaki

**15 typów badges**:
1. First Blood
2. Rising Star (Level 5)
3. Elite Coder (Level 10)
4. Speed Demon
5. Lightning Fast
6. Consistent (7-day streak)
7. Streak Master (30-day streak)
8. DeFi Master
9. NFT Expert
10. Security Specialist
11. Bug Hunter
12. Gas Optimizer
13. Community Helper
14. Perfect Score
15. Early Adopter

### 8. LeaderboardManager

**Cel**: Globalne i kategorialne rankingi

**Funkcje**:
- Global leaderboard (top 100)
- Category leaderboards
- Time-boxed competitions
- Prize distribution dla top scorers

## 💻 Frontend

### Tech Stack

- **React 18** - UI framework
- **Vite** - Build tool
- **Wagmi v2** - React hooks dla Ethereum
- **RainbowKit** - Wallet connection
- **TailwindCSS** - Styling
- **Monaco Editor** - Code editor dla Solidity
- **Framer Motion** - Animations
- **React Router** - Routing

### Strony

1. **Home** - Landing page z info o platformie
2. **Dashboard** - Przegląd progressu użytkownika
3. **Challenges** - Lista wszystkich wyzwań z filtrami
4. **ChallengeDetail** - Szczegóły pojedynczego wyzwania
5. **SolutionEditor** - IDE do pisania i testowania rozwiązań
6. **Profile** - Profil użytkownika z badges i statystykami
7. **Leaderboard** - Globalne rankingi

### Kluczowe komponenty

- **Navbar** - Nawigacja z wallet connection
- **ChallengeCard** - Wyświetlanie wyzwania
- **CodeEditor** - Monaco editor z syntax highlighting
- **ProgressBar** - Wizualizacja postępu
- **BadgeCard** - Wyświetlanie odznak
- **LeaderboardTable** - Tabela rankingowa

## 🚀 Instalacja

### Wymagania

- Node.js >= 18
- npm lub yarn
- MetaMask lub inny wallet Web3

### Krok 1: Clone repo

```bash
git clone https://github.com/yourusername/chainquest.git
cd chainquest
```

### Krok 2: Install dependencies

```bash
# Root dependencies
npm install

# Contracts
cd packages/contracts
npm install

# Frontend
cd ../frontend
npm install
```

### Krok 3: Configure environment

```bash
# W packages/contracts skopiuj .env.example do .env
cp .env.example .env

# Wypełnij zmienne:
PRIVATE_KEY=your_private_key
SEPOLIA_RPC_URL=your_alchemy_or_infura_url
ETHERSCAN_API_KEY=your_etherscan_key
```

## 📦 Deployment

### Local development (Hardhat Network)

```bash
# Terminal 1: Start local blockchain
cd packages/contracts
npx hardhat node

# Terminal 2: Deploy contracts
npx hardhat run scripts/deploy.js --network localhost

# Terminal 3: Start frontend
cd packages/frontend
npm run dev
```

### Testnet (Sepolia)

```bash
cd packages/contracts

# Deploy to Sepolia
npx hardhat run scripts/deploy.js --network sepolia

# Verify contracts
npx hardhat verify --network sepolia <CONTRACT_ADDRESS>
```

### Update frontend config

Po deployment, zaktualizuj adresy kontraktów w:

```javascript
// packages/frontend/src/config/constants.js
export const CONTRACTS = {
  ExperienceToken: '0x...',
  AchievementBadges: '0x...',
  UserProfile: '0x...',
  ChallengeRegistry: '0x...',
  // ... etc
};
```

## 📖 Użytkowani

### Dla Users

1. **Connect Wallet**: Połącz MetaMask lub inny wallet
2. **Mint Profile**: Darmowy mint profilu NFT (jednorazowo)
3. **Browse Challenges**: Przeglądaj dostępne wyzwania
4. **Start Coding**: Wybierz wyzwanie i zacznij kodować
5. **Submit Solution**: Kompiluj, testuj i submituj
6. **Earn Rewards**: Otrzymuj XP i badges automatycznie

### Dla Adminów

```bash
# Add new challenge
npx hardhat run scripts/add-challenge.js --network sepolia

# Create competition
npx hardhat run scripts/create-competition.js --network sepolia

# Manage permissions
npx hardhat run scripts/manage-permissions.js --network sepolia
```

## 🛠️ Rozwój

### Dodawanie nowego challenge

1. W admin panel lub przez skrypt:

```javascript
await challengeRegistry.createChallenge(
  "Challenge Title",
  "Short description",
  "ipfs://QmHash...",  // Full details on IPFS
  1,                   // difficulty (0-4)
  0,                   // category (0-7)
  250,                 // basePoints
  2,                   // requiredLevel
  keccak256("test_criteria")
);
```

2. Przygotuj test suite w SolutionValidator
3. Upload szczegółów do IPFS

### Testowanie kontraktów

```bash
cd packages/contracts
npx hardhat test
npx hardhat coverage
```

### Testing frontendu

```bash
cd packages/frontend
npm run test
npm run build
```

## 🔐 Security

- Wszystkie kontrakty używają **Solady** (gas-optimized libraries)
- Access control przez **Ownable**
- Reentrancy protection gdzie potrzebne
- Soul-bound NFTs (Profile i Badges nie są transferowalne)
- Extensive testing przed production

## 🎨 Customization

### Zmiana level thresholds

W `UserProfile.sol`:

```solidity
levelThresholds[1] = 0;
levelThresholds[2] = 100;  // Zmień wartości
// ...
```

### Dodanie nowego typu badge

```solidity
badgeContract.createBadgeType(
  "New Badge Name",
  "Description",
  "ipfs://image_uri"
);
```

### Modyfikacja reward formula

W `RewardDistributor.sol` zmień multipliers:

```solidity
difficultyMultipliers[0] = 10000;  // 1x
difficultyMultipliers[1] = 15000;  // 1.5x
// ...
```

## 📊 Metryki i Analytics

Platform tracking (off-chain):
- Total users
- Challenges completed
- XP distributed
- Badges minted
- Active competitions

## 🗺️ Roadmap

### Faza 1: MVP (Completed ✅)
- ✅ Core 8 kontraktów
- ✅ Basic frontend
- ✅ Wallet integration
- ✅ Challenge system

### Faza 2: Enhanced Features
- [ ] Oracle integration dla advanced validation
- [ ] IPFS integration dla code storage
- [ ] Hint marketplace
- [ ] Peer review DAO
- [ ] Team challenges

### Faza 3: Community
- [ ] User-submitted challenges
- [ ] Governance token
- [ ] Revenue sharing
- [ ] Educational content marketplace

### Faza 4: Scale
- [ ] L2 deployment (Base, Arbitrum)
- [ ] Mobile app
- [ ] API dla third-party integration
- [ ] White-label solution

## 🤝 Contributing

Contributions are welcome! Zobacz [CONTRIBUTING.md](./CONTRIBUTING.md)

## 📄 License

MIT License - see [LICENSE](./LICENSE)

## 🙏 Acknowledgments

- OpenZeppelin - Smart contract standards
- Solady - Gas-optimized libraries  
- Uniswap - AMM inspiration
- Ethereum Foundation

## 📞 Contact

- Website: https://chainquest.io
- Twitter: @ChainQuestIO
- Discord: discord.gg/chainquest
- Email: team@chainquest.io

---

**Built with ❤️ by the ChainQuest Team**

*Empowering the next generation of Web3 developers* 🚀
