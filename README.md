# 比特币世界的圣经

[中本聪的白皮书(英文)](https://bitcoin.org/bitcoin.pdf) ([中文版 （翻译略有问题，注意识别）](./docs/bitcoin_cn.pdf))

## 论文中提到的参考资料

1. [W. Dai, "b-money," http://www.weidai.com/bmoney.txt, 1998.]()
1. [H. Massias, X.S. Avila, and J.-J. Quisquater, "Design of a secure timestamping service with minimal trust requirements," In 20th Symposium on Information Theory in the Benelux, May 1999.](./docs/10.1.1.13.6228.pdf)
1. [S. Haber, W.S. Stornetta, "How to time-stamp a digital document," In Journal of Cryptology, vol 3, no2, pages 99-111, 1991.](./docs/Haber_Stornetta.pdf)
1. [D. Bayer, S. Haber, W.S. Stornetta, "Improving the efficiency and reliability of digital time-stamping," In Sequences II: Methods in Communication, Security and Computer Science, pages 329-334, 1993.](./docs/10.1.1.71.4891.pdf)
1. [S. Haber, W.S. Stornetta, "Secure names for bit-strings," In Proceedings of the 4th ACM Conference on Computer and Communications Security, pages 28-35, April 1997.](./docs/secure-names-bit-strings.pdf)
1. [A. Back, "Hashcash - a denial of service counter-measure,"](./docs/10.1.1.15.8.pdf)
1. [http://www.hashcash.org/papers/hashcash.pdf, 2002.](./docs/hashcash.pdf)
1. [R.C. Merkle, "Protocols for public key cryptosystems," In Proc. 1980 Symposium on Security and Privacy, IEEE Computer Society, pages 122-133, April 1980.](./docs/Protocols_for_Public_Key_Cryptosystems.pdf)
1. [W. Feller, "An introduction to probability theory and its applications," 1957](./docs/WilliamFeller-AnIntroductionToProbabilityTheoryAndItsApplications.VolII.pdf)

## 中本聪

[中本聪](https://zh.wikipedia.org/wiki/%E4%B8%AD%E6%9C%AC%E8%81%AA) [英文](https://en.wikipedia.org/wiki/Satoshi_Nakamoto)

<!-- [Satoshi Nakamoto: 9 Interesting Facts You Need To Know](https://coinsutra.com/satoshi-nakamoto-facts/) -->

## 有意思的部分

### 创世区块

[创世区块](https://en.bitcoin.it/wiki/Genesis_block)

[创世区块的代码](https://sourceforge.net/p/bitcoin/code/133/tree/trunk/main.cpp#l1628)

```cpp
// Genesis Block:
// GetHash()      = 0x000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f
// hashMerkleRoot = 0x4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b
// txNew.vin[0].scriptSig     = 486604799 4 0x736B6E616220726F662074756F6C69616220646E6F63657320666F206B6E697262206E6F20726F6C6C65636E61684320393030322F6E614A2F33302073656D695420656854
// txNew.vout[0].nValue       = 5000000000
// txNew.vout[0].scriptPubKey = 0x5F1DF16B2B704C8A578D0BBAF74D385CDE12C11EE50455F3C438EF4C3FBCF649B6DE611FEAE06279A60939E028A8D65C10B73071A6F16719274855FEB0FD8A6704 OP_CHECKSIG
// block.nVersion = 1
// block.nTime    = 1231006505
// block.nBits    = 0x1d00ffff
// block.nNonce   = 2083236893
// CBlock(hash=000000000019d6, ver=1, hashPrevBlock=00000000000000, hashMerkleRoot=4a5e1e, nTime=1231006505, nBits=1d00ffff, nNonce=2083236893, vtx=1)
//   CTransaction(hash=4a5e1e, ver=1, vin.size=1, vout.size=1, nLockTime=0)
//     CTxIn(COutPoint(000000, -1), coinbase 04ffff001d0104455468652054696d65732030332f4a616e2f32303039204368616e63656c6c6f72206f6e206272696e6b206f66207365636f6e64206261696c6f757420666f722062616e6b73)
//     CTxOut(nValue=50.00000000, scriptPubKey=0x5F1DF16B2B704C8A578D0B)
//   vMerkleTree: 4a5e1e
```

最新的代码中已经见不到上述内容了，创始区块在源代码[init.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/init.cpp)通过`LoadGenesisBlock(chainparams);`加载。而该函数定义在[validation.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/validation.cpp)中。创始区块定义在[chainparams.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/chainparams.cpp)中，如下所示：

```cpp
...

genesis = CreateGenesisBlock(1231006505, 2083236893, 0x1d00ffff, 1, 50 * COIN);
consensus.hashGenesisBlock = genesis.GetHash();
assert(consensus.hashGenesisBlock == uint256S("0x000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f"));
assert(genesis.hashMerkleRoot == uint256S("0x4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"));

...

/**
 * Build the genesis block. Note that the output of its generation
 * transaction cannot be spent since it did not originally exist in the
 * database.
 *
 * CBlock(hash=000000000019d6, ver=1, hashPrevBlock=00000000000000, hashMerkleRoot=4a5e1e, nTime=1231006505, nBits=1d00ffff, nNonce=2083236893, vtx=1)
 *   CTransaction(hash=4a5e1e, ver=1, vin.size=1, vout.size=1, nLockTime=0)
 *     CTxIn(COutPoint(000000, -1), coinbase 04ffff001d0104455468652054696d65732030332f4a616e2f32303039204368616e63656c6c6f72206f6e206272696e6b206f66207365636f6e64206261696c6f757420666f722062616e6b73)
 *     CTxOut(nValue=50.00000000, scriptPubKey=0x5F1DF16B2B704C8A578D0B)
 *   vMerkleTree: 4a5e1e
 */
static CBlock CreateGenesisBlock(uint32_t nTime, uint32_t nNonce, uint32_t nBits, int32_t nVersion, const CAmount& genesisReward)
{
    const char* pszTimestamp = "The Times 03/Jan/2009 Chancellor on brink of second bailout for banks";
    const CScript genesisOutputScript = CScript() << ParseHex("04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f") << OP_CHECKSIG;
    return CreateGenesisBlock(pszTimestamp, genesisOutputScript, nTime, nNonce, nBits, nVersion, genesisReward);
}
```

[创世区块的钱去了哪里？](https://blockchain.info/address/1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa)

### 比特币总量受控

[受控的货币](https://en.bitcoin.it/wiki/Controlled_supply)

```
Bitcoins are created each time a user discovers a new block. The rate of block creation is adjusted every 2016 blocks to aim for a constant two week adjustment period (equivalent to 6 per hour.) The number of bitcoins generated per block is set to decrease geometrically, with a 50% reduction every 210,000 blocks, or approximately four years. The result is that the number of bitcoins in existence will not exceed slightly less than 21 million.[2] Speculated justifications for the unintuitive value "21 million" are that it matches a 4-year reward halving schedule; or the ultimate total number of Satoshis that will be mined is close to the maximum capacity of a 64-bit floating point number. Satoshi has never really justified or explained many of these constants.
```

上述参数均定义在[chainparams.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/chainparams.cpp)中，如下所示：

```cpp
consensus.nSubsidyHalvingInterval = 210000;
consensus.nPowTargetTimespan = 14 * 24 * 60 * 60; // two weeks
consensus.nPowTargetSpacing = 10 * 60;
consensus.nRuleChangeActivationThreshold = 1916; // 95% of 2016
consensus.nMinerConfirmationWindow = 2016; // nPowTargetTimespan / nPowTargetSpacing
```

### 为什么说中本聪持有100万比特币

* [How do we know/ guess how many BTC are owned by Satoshi?](https://www.quora.com/How-do-we-know-guess-how-many-BTC-are-owned-by-Satoshi)
* [The Well Deserved Fortune of Satoshi Nakamoto, Bitcoin creator, Visionary and Genius](https://bitslog.wordpress.com/2013/04/17/the-well-deserved-fortune-of-satoshi-nakamoto/)
* [The mysterious creator of bitcoin is sitting on a $700 million fortune](http://uk.businessinsider.com/satoshi-nakamoto-owns-one-million-bitcoin-700-price-2016-6)
* [Tell me why Satoshi Nakamoto didn't spend a Satoshi from his 1 Mio BTC](https://bitcointalk.org/index.php?topic=1382380.0)
* [9-most-famous-bitcoin-addresses](http://www.theopenledger.com/9-most-famous-bitcoin-addresses/)

### 如何追踪比特币交易

[Tracking Bitcoin Transactions on the Blockchain](https://www.sans.org/summit-archives/file/summit-archive-1498165491.pdf)

# 比特币

* [https://bitcoincore.org/](https://bitcoincore.org/)
* [比特币社区](https://bitcoin.org/)
* [查看比特币的区块信息 (Block #0)](https://blockexplorer.com/block/000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f)
