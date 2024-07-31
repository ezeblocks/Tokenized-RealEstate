// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract RealEstateToken is ERC20, Ownable {

    uint256 public constant INITIAL_SUPPLY = 100000 * (10 ** uint256(decimals()));
    bool public mintingFinished = false;

    event MintFinished();

    constructor() ERC20("RealEstateToken", "RET") Ownable() {
        _mint(msg.sender, INITIAL_SUPPLY);
    }

    modifier canMint() {
        require(!mintingFinished, "Minting is finished");
        _;
    }

    function finishMinting() public onlyOwner canMint returns (bool) {
        mintingFinished = true;
        emit MintFinished();
        return true;
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }
}
