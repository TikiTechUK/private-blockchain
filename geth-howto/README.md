# Geth How To

## Send someone some Ether

First unlock your account (don't forget to replace the address and password)

personal.unlockAccount("0xdd2f7a239511cb657b0a230d6b3ab6f1177e0b36", "password")


Then send your Ether

eth.sendTransaction({from:"0xdd2f7a239511cb657b0a230d6b3ab6f1177e0b36", to:"0x8e87634b6083eb35fc680f894dabacf52cde3d20", value: 20})
