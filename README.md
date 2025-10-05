# FHE_1.0
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "fhevm/lib/TFHE.sol";

contract ConfidentialCounter {
    euint8 private counter;
    event ValueStored(bytes value);

    function storeCounter(bytes calldata encryptedValue) public {
        counter = TFHE.asEuint8(encryptedValue);
        emit ValueStored(encryptedValue);
    }

    // --- NUEVA FUNCIÓN PARA LA V2 ---
    /**
     * @dev Incrementa el contador en 1 de forma homomórfica.
     */
    function increment() public {
        // TFHE.add() suma dos valores encriptados.
        // Aquí sumamos el contador actual con un '1' encriptado.
        counter = TFHE.add(counter, TFHE.asEuint8(1));
    }
    // --- FIN DE LA NUEVA FUNCIÓN ---

    function isOver18() public view returns (ebool) {
        euint8 valueToCompare = TFHE.asEuint8(18);
        ebool isOver = TFHE.gt(counter, valueToCompare);
        return isOver;
    }
}
