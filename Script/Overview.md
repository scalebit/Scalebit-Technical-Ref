## 比特币脚本
随着Taproot升级，比特币脚本现在可以通过一个默克尔树，指定其中任意一个分支中的脚本进行执行。这为建立新的，更加复杂的脚本，以及对不同的脚本类型进行结合都提供了有效的帮助。下文中将其统称为Tapscript。

### Tapscript编写环境

实际上，Tapscript和一般的比特币script并无区别，区别仅在于其能够选择多个脚本文件作为叶子节点。因此存在两种可能的编辑方法。
- 需要熟悉比特币的基础opcode，这是任何脚本的基础：https://en.bitcoin.it/wiki/Script。
- 推荐使用qed搭建的本地比特币脚本解析器和IDE：https://bitide.qedprotocol.com/。

## 较为重要的比特币脚本类型
在Tapscript诞生前后，特定类型的比特币脚本都具备重要的作用。其中比较具有代表性的为HTLC，PTLC。在Taproot更新之后，Multi-Sig和DLC都变的更加易于实现了。与此同时也带来了铭文这种专门服务于L2的脚本类型。

### HTLC
参考Bip-0199

    OP_IF
        [HASHOP] <digest> OP_EQUALVERIFY OP_DUP OP_HASH160 <seller pubkey hash>            
    OP_ELSE
        <num> [TIMEOUTOP] OP_DROP OP_DUP OP_HASH160 <buyer pubkey hash>
    OP_ENDIF
    OP_EQUALVERIFY
    OP_CHECKSIG

一些其他的衍生版本：
- pay-to-sudoko: https://bitcoincore.org/en/2016/02/26/zero-knowledge-contingent-payments-announcement/

### PTLC

需要进行实现，目前存在的参考

    https://github.com/GeneFerneau/teleport-transactions/tree/ptlc

思路

    https://gist.github.com/chris-belcher/9144bd57a91c194e332fb5ca371d0964#ecdsa-2p

### M-to-n Multi-Sig

Bip-11当中的m-n的Multi-Sig检查

    M [pubkey1] [pubkey2] ... [pubkeyn] N OP_CHECKMULTISIG

基于Schnorr签名的Multi-Sig

    <32-byte pubkey1> OP_CHECKSIG <32-byte pubkey2> OP_CHECKSIGADD 1 OP_NUMEQUAL

具体需要按照Bip-341进行构造

    https://github.com/bitcoin/bips/blob/master/bip-0341.mediawiki#constructing-and-spending-taproot-outputs

其他参考

    https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2022-August/020877.html

### DLC

DLC范式不存在Bip

实现参考

    https://github.com/p2pderivatives/dlc
    https://github.com/p2pderivatives/rust-dlc

验证

    https://github.com/discreetlogcontracts/dlcspecs/blob/master/Introduction.md

### Inscription

Inscription不存在Bip

解析器

    https://github.com/balletcrypto/bitcoin-inscription-parser

格式

    OP_FALSE
    OP_IF
    OP_PUSH "ord"
    OP_PUSH 1
    OP_PUSH "text/plain;charset=utf-8"
    OP_PUSH 0
    OP_PUSH "Hello, world!"
    OP_ENDIF

