# Implement

{% hint style="info" %}
* Project setup
  * Make a library crate with melprot as a dep
* Lookups
  * Easiest part
  * Decode the gibbername as a block height and index
  * Use that to find the&#x20;
* Registration
  * Need to send a transaction of a particular shape
  * Problem 1: need to prompt the user to send using wallet
    * Solution: wallet URL
  * Problem 2: need to know when the transaction has left the wallet
    * Solution: monitor for activity on the wallet
* Transfers
  * Just transfer the "NFT"
  * Similar approach to registration. Left to the reader (complete code on GitHub)
{% endhint %}
