# wLoki-smart-contract
The ERC20 contract for wLoki

This contract is based on the highly audited wBTC smart contract. Uncompiled source for the wBTC smart contract can be found at https://github.com/WrappedBTC/bitcoin-token-smart-contracts

This is a single file version of the same contract, with the following edits:
~~~
- event Burn(address indexed burner, uint256 value);
+ event Burn(address indexed burner, uint256 value, string note);

function burn(uint256 _value) public {
- _burn(msg.sender, _value);
+ _burn(msg.sender, _value, "");
}

+  function burnWithNote(uint256 _value, string memory _note) public {
+    _burn(msg.sender, _value, _note);
+  }

- function _burn(address _who, uint256 _value) internal {
+ function _burn(address _who, uint256 _value, string memory _note) internal {
    require(_value <= balances[_who]);
    // no need to require value <= totalSupply, since that would imply the
    // sender's balance is greater than the totalSupply, which *should* be an assertion failure

    balances[_who] = balances[_who].sub(_value);
    totalSupply_ = totalSupply_.sub(_value);
-    emit Burn(_who, _value);
+    emit Burn(_who, _value, _note);
    emit Transfer(_who, address(0), _value);
  }
~~~ 
These changes simply allow for the inclusion of a unique string which is used to detect which Loki address to credit when burning wLoki on the Ethereum chain.
