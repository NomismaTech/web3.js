.. _eth-contract:

========
web3.eth.Contract
========

The ``web3.eth.Contract`` object makes it easy to interact with smart contracts on the ethereum blockchain.
When you create a new contract object you give it the json interface of the respective smart contract
and web3 will auto convert all calls into low level ABI calls over RPC for you.

This allows you to interact with smart contracts as if they were JavaScript objects.

To use it standalone:

.. code-block:: javascript
    var Contract = require('web3-eth-contract');

    // set provider for all later instances to use
    Contract.setProvider('ws://localhost:8546');

    var contract = new Contract(jsonInterface, address);

    contract.methods.somFunc().send({from: ....})
    .on('receipt', function(){
        ...
    });


------------------------------------------------------------------------------


new contract
=========

.. index:: JSON interface

.. code-block:: javascript

    new web3.eth.Contract(jsonInterface[, address][, options])

Creates a new contract instance with all its methods and events defined in its :ref:`json interface <glossary-json-interface>` object.

----------
Parameters
----------

1. ``jsonInterface`` - ``Object``: The json interface for the contract to instantiate
2. ``address`` - ``String`` (optional): The address of the smart contract to call.
3. ``options`` - ``Object`` (optional): The options of the contract. Some are used as fallbacks for calls and transactions:
    * ``from`` - ``String``: The address transactions should be made from.
    * ``gasPrice`` - ``String``: The gas price in wei to use for transactions.
    * ``gas`` - ``Number``: The maximum gas provided for a transaction (gas limit).
    * ``data`` - ``String``: The byte code of the contract. Used when the contract gets :ref:`deployed <contract-deploy>`.

-------
Returns
-------

``Object``: The contract instance with all its methods and events.


-------
Example
-------

.. code-block:: javascript

    var myContract = new web3.eth.Contract([...], '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe', {
        from: '0x1234567890123456789012345678901234567891', // default from address
        gasPrice: '20000000000' // default gas price in wei, 20 gwei in this case
    });


------------------------------------------------------------------------------


= Properties =
=========

------------------------------------------------------------------------------

.. _eth-contract-defaultaccount

defaultAccount
=====================

.. code-block:: javascript

    web3.eth.Contract.defaultAccount
    contract.defaultAccount // on contract instance

This default address is used as the default ``"from"`` property, if no ``"from"`` property is specified in for the following methods:

- :ref:`web3.eth.sendTransaction() <eth-sendtransaction>`
- :ref:`web3.eth.call() <eth-call>`
- :ref:`new web3.eth.Contract() -> myContract.methods.myMethod().call() <eth-contract-call>`
- :ref:`new web3.eth.Contract() -> myContract.methods.myMethod().send() <eth-contract-send>`

--------
Property
--------


``String`` - 20 Bytes: Any ethereum address. You should have the private key for that address in your node or keystore. (Default is ``undefined``)


-------
Example
-------


.. code-block:: javascript

    web3.eth.defaultAccount;
    > undefined

    // set the default account
    web3.eth.defaultAccount = '0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe';


------------------------------------------------------------------------------

.. _eth-contract-defaultblock:

defaultBlock
=====================

.. code-block:: javascript

    web3.eth.Contract.defaultBlock
    contract.defaultBlock // on contract instance

The default block is used for certain methods. You can override it by passing in the defaultBlock as last parameter.
The default value of it is "latest".

----------
Property
----------


The default block parameters can be one of the following:

- ``Number|BN|BigNumber``: A block number
- ``"genesis"`` - ``String``: The genesis block
- ``"latest"`` - ``String``: The latest block (current head of the blockchain)
- ``"pending"`` - ``String``: The currently mined block (including pending transactions)
- ``"earliest"`` - ``String``: The genesis block

Default is ``"latest"``


-------
Example
-------

.. code-block:: javascript

    contract.defaultBlock;
    > "latest"

    // set the default block
    contract.defaultBlock = 231;



------------------------------------------------------------------------------

.. _eth-contract-defaulthardfork:

defaultHardfork
=====================

.. code-block:: javascript

    contract.defaultHardfork

The default hardfork property is used for signing transactions locally.

----------
Property
----------


The default hardfork property can be one of the following:

- ``"chainstart"`` - ``String``
- ``"homestead"`` - ``String``
- ``"dao"`` - ``String``
- ``"tangerineWhistle"`` - ``String``
- ``"spuriousDragon"`` - ``String``
- ``"byzantium"`` - ``String``
- ``"constantinople"`` - ``String``
- ``"petersburg"`` - ``String``
- ``"istanbul"`` - ``String``

Default is ``"petersburg"``


-------
Example
-------

.. code-block:: javascript

    contract.defaultHardfork;
    > "petersburg"

    // set the default block
    contract.defaultHardfork = 'istanbul';


------------------------------------------------------------------------------

.. _eth-contract-defaultchain:

defaultChain
=====================

.. code-block:: javascript

    contract.defaultChain

The default chain property is used for signing transactions locally.

----------
Property
----------


The default chain property can be one of the following:

- ``"mainnet"`` - ``String``
- ``"goerli"`` - ``String``
- ``"kovan"`` - ``String``
- ``"rinkeby"`` - ``String``
- ``"ropsten"`` - ``String``

Default is ``"mainnet"``


-------
Example
-------

.. code-block:: javascript

    contract.defaultChain;
    > "mainnet"

    // set the default chain
    contract.defaultChain = 'goerli';


------------------------------------------------------------------------------

.. _eth-contract-defaultcommon:

defaultCommon
=====================

.. code-block:: javascript

    contract.defaultCommon

The default common property is used for signing transactions locally.

----------
Property
----------


The default common property does contain the following ``Common`` object:

- ``customChain`` - ``Object``: The custom chain properties
    - ``name`` - ``string``: (optional) The name of the chain
    - ``networkId`` - ``number``: Network ID of the custom chain
    - ``chainId`` - ``number``: Chain ID of the custom chain
- ``baseChain`` - ``string``: (optional) ``mainnet``, ``goerli``, ``kovan``, ``rinkeby``, or ``ropsten``
- ``hardfork`` - ``string``: (optional) ``chainstart``, ``homestead``, ``dao``, ``tangerineWhistle``, ``spuriousDragon``, ``byzantium``, ``constantinople``, ``petersburg``, or ``istanbul``


Default is ``undefined``.


-------
Example
-------

.. code-block:: javascript

    contract.defaultCommon;
    > {customChain: {name: 'custom-network', chainId: 1, networkId: 1}, baseChain: 'mainnet', hardfork: 'petersburg'}

    // set the default common
    contract.defaultCommon = {customChain: {name: 'custom-network', chainId: 1, networkId: 1}, baseChain: 'mainnet', hardfork: 'petersburg'};


------------------------------------------------------------------------------

.. _eth-contract-transactionblocktimeout:

transactionBlockTimeout
=====================

.. code-block:: javascript

    web3.eth.Contract.transcationBlockTimeout
    contract.transactionBlockTimeout // on contract instance

The ``transactionBlockTimeout`` will be used over a socket based connection. This option does define the amount of new blocks it should wait until the first confirmation happens.
This means the PromiEvent rejects with a timeout error when the timeout got exceeded.


-------
Returns
-------

``number``: The current value of transactionBlockTimeout (default: 50)

------------------------------------------------------------------------------

.. _eth-contract-module-transactionconfirmationblocks:

transactionConfirmationBlocks
=====================

.. code-block:: javascript

    web3.eth.Contract.transactionConfirmationBlocks
    contract.transactionConfirmationBlocks // on contract instance

This defines the number of blocks it requires until a transaction will be handled as confirmed.


-------
Returns
-------

``number``: The current value of transactionConfirmationBlocks (default: 24)

------------------------------------------------------------------------------

.. _eth-contract-module-transactionpollingtimeout:

transactionPollingTimeout
=====================

.. code-block:: javascript

    web3.eth.Contract.transactionPollingTimeout
    contract.transactionPollingTimeout // on contract instance

The ``transactionPollingTimeout``  will be used over a HTTP connection.
This option defines the number of seconds Web3 will wait for a receipt which confirms that a transaction was mined by the network. NB: If this method times out, the transaction may still be pending.


-------
Returns
-------

``number``: The current value of transactionPollingTimeout (default: 750)

------------------------------------------------------------------------------

options
=========

.. code-block:: javascript

    myContract.options

The options ``object`` for the contract instance. ``from``, ``gas`` and ``gasPrice`` are used as fallback values when sending transactions.

-------
Properties
-------

``Object`` - options:

- ``address`` - ``String``: The address where the contract is deployed. See :ref:`options.address <contract-address>`.
- ``jsonInterface`` - ``Array``: The json interface of the contract. See :ref:`options.jsonInterface <contract-json-interface>`.
- ``data`` - ``String``: The byte code of the contract. Used when the contract gets :ref:`deployed <contract-deploy>`.
- ``from`` - ``String``: The address transactions should be made from.
- ``gasPrice`` - ``String``: The gas price in wei to use for transactions.
- ``gas`` - ``Number``: The maximum gas provided for a transaction (gas limit).


-------
Example
-------

.. code-block:: javascript

    myContract.options;
    > {
        address: '0x1234567890123456789012345678901234567891',
        jsonInterface: [...],
        from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe',
        gasPrice: '10000000000000',
        gas: 1000000
    }

    myContract.options.from = '0x1234567890123456789012345678901234567891'; // default from address
    myContract.options.gasPrice = '20000000000000'; // default gas price in wei
    myContract.options.gas = 5000000; // provide as fallback always 5M gas


------------------------------------------------------------------------------

.. _contract-address:

options.address
=========

.. code-block:: javascript

    myContract.options.address

The address used for this contract instance.
All transactions generated by web3.js from this contract will contain this address as the "to".

The address will be stored in lowercase.


-------
Property
-------

``address`` - ``String|null``: The address for this contract, or ``null`` if it's not yet set.


-------
Example
-------

.. code-block:: javascript

    myContract.options.address;
    > '0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae'

    // set a new address
    myContract.options.address = '0x1234FFDD...';


------------------------------------------------------------------------------

.. _contract-json-interface:

options.jsonInterface
=========

.. code-block:: javascript

    myContract.options.jsonInterface

The :ref:`json interface <glossary-json-interface>` object derived from the `ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`_ of this contract.


-------
Property
-------

``jsonInterface`` - ``Array``: The :ref:`json interface <glossary-json-interface>` for this contract. Re-setting this will regenerate the methods and events of the contract instance.


-------
Example
-------

.. code-block:: javascript

    myContract.options.jsonInterface;
    > [{
        "type":"function",
        "name":"foo",
        "inputs": [{"name":"a","type":"uint256"}],
        "outputs": [{"name":"b","type":"address"}]
    },{
        "type":"event",
        "name":"Event",
        "inputs": [{"name":"a","type":"uint256","indexed":true},{"name":"b","type":"bytes32","indexed":false}],
    }]

    // set a new interface
    myContract.options.jsonInterface = [...];


------------------------------------------------------------------------------


= Methods =
=========


------------------------------------------------------------------------------

clone
=====================

.. code-block:: javascript

    myContract.clone()

Clones the current contract instance.

----------
Parameters
----------

none

-------
Returns
-------


``Object``: The new contract instance.

-------
Example
-------

.. code-block:: javascript

    var contract1 = new eth.Contract(abi, address, {gasPrice: '12345678', from: fromAddress});

    var contract2 = contract1.clone();
    contract2.options.address = address2;

    (contract1.options.address !== contract2.options.address);
    > true

------------------------------------------------------------------------------


.. _contract-deploy:

.. index:: contract deploy

deploy
=====================

.. code-block:: javascript

    myContract.deploy(options)

Call this function to deploy the contract to the blockchain.
After successful deployment the promise will resolve with a new contract instance.

----------
Parameters
----------

1. ``options`` - ``Object``: The options used for deployment.
    * ``data`` - ``String``: The byte code of the contract.
    * ``arguments`` - ``Array`` (optional): The arguments which get passed to the constructor on deployment.

-------
Returns
-------


``Object``: The transaction object:

- ``Array`` - arguments: The arguments passed to the method before. They can be changed.
- ``Function`` - :ref:`send <contract-send>`: Will deploy the contract. The promise will resolve with the new contract instance, instead of the receipt!
- ``Function`` - :ref:`estimateGas <contract-estimateGas>`: Will estimate the gas used for deploying.
- ``Function`` - :ref:`encodeABI <contract-encodeABI>`: Encodes the ABI of the deployment, which is contract data + constructor parameters

 For details to the methods see the documentation below.

-------
Example
-------

.. code-block:: javascript

    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .send({
        from: '0x1234567890123456789012345678901234567891',
        gas: 1500000,
        gasPrice: '30000000000000'
    }, function(error, transactionHash){ ... })
    .on('error', function(error){ ... })
    .on('transactionHash', function(transactionHash){ ... })
    .on('receipt', function(receipt){
       console.log(receipt.contractAddress) // contains the new contract address
    })
    .on('confirmation', function(confirmationNumber, receipt){ ... })
    .then(function(newContractInstance){
        console.log(newContractInstance.options.address) // instance with the new contract address
    });


    // When the data is already set as an option to the contract itself
    myContract.options.data = '0x12345...';

    myContract.deploy({
        arguments: [123, 'My String']
    })
    .send({
        from: '0x1234567890123456789012345678901234567891',
        gas: 1500000,
        gasPrice: '30000000000000'
    })
    .then(function(newContractInstance){
        console.log(newContractInstance.options.address) // instance with the new contract address
    });


    // Simply encoding
    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .encodeABI();
    > '0x12345...0000012345678765432'


    // Gas estimation
    myContract.deploy({
        data: '0x12345...',
        arguments: [123, 'My String']
    })
    .estimateGas(function(err, gas){
        console.log(gas);
    });

------------------------------------------------------------------------------


methods
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]])

Creates a transaction object for that method, which then can be :ref:`called <contract-call>`, :ref:`send <contract-send>`, :ref:`estimated  <contract-estimateGas>`or :ref:`ABI encoded <contract-encodeABI>`.

The methods of this smart contract are available through:

- The name: ``myContract.methods.myMethod(123)``
- The name with parameters: ``myContract.methods['myMethod(uint256)'](123)``
- The signature: ``myContract.methods['0x58cf5f10'](123)``

This allows calling functions with same name but different parameters from the JavaScript contract object.

----------
Parameters
----------

Parameters of any method depend on the smart contracts methods, defined in the :ref:`JSON interface <glossary-json-interface>`.

-------
Returns
-------

``Object``: The transaction object:

- ``Array`` - arguments: The arguments passed to the method before. They can be changed.
- ``Function`` - :ref:`call <contract-call>`: Will call the "constant" method and execute its smart contract method in the EVM without sending a transaction (Can't alter the smart contract state).
- ``Function`` - :ref:`send <contract-send>`: Will send a transaction to the smart contract and execute its method (Can alter the smart contract state).
- ``Function`` - :ref:`estimateGas <contract-estimateGas>`: Will estimate the gas used when the method would be executed on chain.
- ``Function`` - :ref:`encodeABI <contract-encodeABI>`: Encodes the ABI for this method. This can be send using a transaction, call the method or passing into another smart contracts method as argument.

 For details to the methods see the documentation below.

-------
Example
-------

.. code-block:: javascript

    // calling a method

    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, result){
        ...
    });

    // or sending and using a promise
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(receipt){
        // receipt can also be a new contract instance, when coming from a "contract.deploy({...}).send()"
    });

    // or sending and using the events

    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .on('transactionHash', function(hash){
        ...
    })
    .on('receipt', function(receipt){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('error', function(error, receipt) {
        ...
    });


------------------------------------------------------------------------------


.. _contract-call:

methods.myMethod.call
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).call(options[, callback])

Will call a "constant" method and execute its smart contract method in the EVM without sending any transaction. Note calling can not alter the smart contract state.

----------
Parameters
----------

1. ``options`` - ``Object`` (optional): The options used for calling.
    * ``from`` - ``String`` (optional): The address the call "transaction" should be made from.
    * ``gasPrice`` - ``String`` (optional): The gas price in wei to use for this call "transaction".
    * ``gas`` - ``Number`` (optional): The maximum gas provided for this call "transaction" (gas limit).
2. ``callback`` - ``Function`` (optional): This callback will be fired with the result of the smart contract method execution as the second argument, or with an error object as the first argument.

-------
Returns
-------

``Promise`` returns ``Mixed``: The return value(s) of the smart contract method.
If it returns a single value, it's returned as is. If it has multiple return values they are returned as an object with properties and indices:

-------
Example
-------

.. code-block:: javascript

    // using the callback
    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, result){
        ...
    });

    // using the promise
    myContract.methods.myMethod(123).call({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(result){
        ...
    });


    // MULTI-ARGUMENT RETURN:

    // Solidity
    contract MyContract {
        function myFunction() returns(uint256 myNumber, string myString) {
            return (23456, "Hello!%");
        }
    }

    // web3.js
    var MyContract = new web3.eth.Contract(abi, address);
    MyContract.methods.myFunction().call()
    .then(console.log);
    > Result {
        myNumber: '23456',
        myString: 'Hello!%',
        0: '23456', // these are here as fallbacks if the name is not know or given
        1: 'Hello!%'
    }


    // SINGLE-ARGUMENT RETURN:

    // Solidity
    contract MyContract {
        function myFunction() returns(string myString) {
            return "Hello!%";
        }
    }

    // web3.js
    var MyContract = new web3.eth.Contract(abi, address);
    MyContract.methods.myFunction().call()
    .then(console.log);
    > "Hello!%"



------------------------------------------------------------------------------


.. _contract-send:

methods.myMethod.send
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).send(options[, callback])

Will send a transaction to the smart contract and execute its method. Note this can alter the smart contract state.

----------
Parameters
----------

1. ``options`` - ``Object``: The options used for sending.
    * ``from`` - ``String``: The address the transaction should be sent from.
    * ``gasPrice`` - ``String`` (optional): The gas price in wei to use for this transaction.
    * ``gas`` - ``Number`` (optional): The maximum gas provided for this transaction (gas limit).
    * ``value`` - ``Number|String|BN|BigNumber``(optional): The value transferred for the transaction in wei.
2. ``callback`` - ``Function`` (optional): This callback will be fired first with the "transactionHash", or with an error object as the first argument.

-------
Returns
-------

The **callback** will return the 32 bytes transaction hash.

``PromiEvent``: A :ref:`promise combined event emitter <promiEvent>`. Will be resolved when the transaction *receipt* is available, OR if this ``send()`` is called from a ``someContract.deploy()``, then the promise will resolve with the *new contract instance*. Additionally the following events are available:

- ``"transactionHash"`` returns ``String``: is fired right after the transaction is sent and a transaction hash is available.
- ``"receipt"`` returns ``Object``: is fired when the transaction *receipt* is available. Receipts from contracts will have no ``logs`` property, but instead an ``events`` property with event names as keys and events as properties. See :ref:`getPastEvents return values <contract-events-return>` for details about the returned event object.
- ``"confirmation"`` returns ``Number``, ``Object``: is fired for every confirmation up to the 24th confirmation. Receives the confirmation number as the first and the receipt as the second argument. Fired from confirmation 1 on, which is the block where it's minded.
- ``"error"`` returns ``Error`` and ``Object|undefined``: Is fired if an error occurs during sending. If the transaction was rejected by the network with a receipt, the second parameter will be the receipt.


-------
Example
-------

.. code-block:: javascript

    // using the callback
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'}, function(error, transactionHash){
        ...
    });

    // using the promise
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(receipt){
        // receipt can also be a new contract instance, when coming from a "contract.deploy({...}).send()"
    });


    // using the event emitter
    myContract.methods.myMethod(123).send({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .on('transactionHash', function(hash){
        ...
    })
    .on('confirmation', function(confirmationNumber, receipt){
        ...
    })
    .on('receipt', function(receipt){
        // receipt example
        console.log(receipt);
        > {
            "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
            "transactionIndex": 0,
            "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
            "blockNumber": 3,
            "contractAddress": "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe",
            "cumulativeGasUsed": 314159,
            "gasUsed": 30234,
            "events": {
                "MyEvent": {
                    returnValues: {
                        myIndexedParam: 20,
                        myOtherIndexedParam: '0x123456789...',
                        myNonIndexParam: 'My String'
                    },
                    raw: {
                        data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
                        topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
                    },
                    event: 'MyEvent',
                    signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                    logIndex: 0,
                    transactionIndex: 0,
                    transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
                    blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
                    blockNumber: 1234,
                    address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
                },
                "MyOtherEvent": {
                    ...
                },
                "MyMultipleEvent":[{...}, {...}] // If there are multiple of the same event, they will be in an array
            }
        }
    })
    .on('error', function(error, receipt) { // If the transaction was rejected by the network with a receipt, the second parameter will be the receipt.
        ...
    });

------------------------------------------------------------------------------


.. _contract-estimateGas:

methods.myMethod.estimateGas
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).estimateGas(options[, callback])

Will call estimate the gas a method execution will take when executed in the EVM without.
The estimation can differ from the actual gas used when later sending a transaction, as the state of the smart contract can be different at that time.

----------
Parameters
----------

1. ``options`` - ``Object`` (optional): The options used for calling.
    * ``from`` - ``String`` (optional): The address the call "transaction" should be made from.
    * ``gas`` - ``Number`` (optional): The maximum gas provided for this call "transaction" (gas limit). Setting a specific value helps to detect out of gas errors. If all gas is used it will return the same number.
    * ``value`` - ``Number|String|BN|BigNumber``(optional): The value transferred for the call "transaction" in wei.
2. ``callback`` - ``Function`` (optional): This callback will be fired with the result of the gas estimation as the second argument, or with an error object as the first argument.

-------
Returns
-------

``Promise`` returns ``Number``: The gas amount estimated.

-------
Example
-------

.. code-block:: javascript

    // using the callback
    myContract.methods.myMethod(123).estimateGas({gas: 5000000}, function(error, gasAmount){
        if(gasAmount == 5000000)
            console.log('Method ran out of gas');
    });

    // using the promise
    myContract.methods.myMethod(123).estimateGas({from: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'})
    .then(function(gasAmount){
        ...
    })
    .catch(function(error){
        ...
    });


------------------------------------------------------------------------------


.. _contract-encodeABI:

methods.myMethod.encodeABI
=====================

.. code-block:: javascript

    myContract.methods.myMethod([param1[, param2[, ...]]]).encodeABI()

Encodes the ABI for this method. This can be used to send a transaction, call a method, or pass it into another smart contracts method as arguments.


----------
Parameters
----------

none

-------
Returns
-------

``String``: The encoded ABI byte code to send via a transaction or call.

-------
Example
-------

.. code-block:: javascript

    myContract.methods.myMethod(123).encodeABI();
    > '0x58cf5f1000000000000000000000000000000000000000000000000000000000000007B'


------------------------------------------------------------------------------


= Events =
=========


------------------------------------------------------------------------------


once
=====================

.. code-block:: javascript

    myContract.once(event[, options], callback)

Subscribes to an event and unsubscribes immediately after the first event or error. Will only fire for a single event.

----------
Parameters
----------

1. ``event`` - ``String``: The name of the event in the contract, or ``"allEvents"`` to get all events.
2. ``options`` - ``Object`` (optional): The options used for deployment.
    * ``filter`` - ``Object`` (optional): Lets you filter events by indexed parameters, e.g. ``{filter: {myNumber: [12,13]}}`` means all events where "myNumber" is 12 or 13.
    * ``topics`` - ``Array`` (optional): This allows you to manually set the topics for the event filter. If given the filter property and event signature, (topic[0]) will not be set automatically.
3. ``callback`` - ``Function``: This callback will be fired for the first *event* as the second argument, or an error as the first argument. See :ref:`getPastEvents return values <contract-events-return>` for details about the event structure.

-------
Returns
-------

``undefined``

-------
Example
-------

.. code-block:: javascript

    myContract.once('MyEvent', {
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
        fromBlock: 0
    }, function(error, event){ console.log(event); });

    // event output example
    > {
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }


------------------------------------------------------------------------------

.. _contract-events:

events
=====================

.. code-block:: javascript

    myContract.events.MyEvent([options][, callback])

Subscribe to an event

----------
Parameters
----------

1. ``options`` - ``Object`` (optional): The options used for deployment.
    * ``filter`` - ``Object`` (optional): Let you filter events by indexed parameters, e.g. ``{filter: {myNumber: [12,13]}}`` means all events where "myNumber" is 12 or 13.
    * ``fromBlock`` - ``Number|String|BN|BigNumber`` (optional): The block number (greater than or equal to) from which to get events on. Pre-defined block numbers as ``"latest"``, ``"earlist"``, ``"pending"``, and ``"genesis"`` can also be used.
    * ``topics`` - ``Array`` (optional): This allows to manually set the topics for the event filter. If given the filter property and event signature, (topic[0]) will not be set automatically.
2. ``callback`` - ``Function`` (optional): This callback will be fired for each *event* as the second argument, or an error as the first argument.

.. _contract-events-return:

-------
Returns
-------

``EventEmitter``: The event emitter has the following events:

- ``"data"`` returns ``Object``: Fires on each incoming event with the event object as argument.
- ``"changed"`` returns ``Object``: Fires on each event which was removed from the blockchain. The event will have the additional property ``"removed: true"``.
- ``"error"`` returns ``Object``: Fires when an error in the subscription occours.
- ``"connected"`` returns ``String``: Fires once after the subscription successfully connected. Returns the subscription id.


The structure of the returned event ``Object`` looks as follows:

- ``event`` - ``String``: The event name.
- ``signature`` - ``String|Null``: The event signature, ``null`` if it's an anonymous event.
- ``address`` - ``String``: Address this event originated from.
- ``returnValues`` - ``Object``: The return values coming from the event, e.g. ``{myVar: 1, myVar2: '0x234...'}``.
- ``logIndex`` - ``Number``: Integer of the event index position in the block.
- ``transactionIndex`` - ``Number``: Integer of the transaction's index position the event was created in.
- ``transactionHash`` 32 Bytes - ``String``: Hash of the transaction this event was created in.
- ``blockHash`` 32 Bytes - ``String``: Hash of the block this event was created in. ``null`` when it's still pending.
- ``blockNumber`` - ``Number``: The block number this log was created in. ``null`` when still pending.
- ``raw.data`` - ``String``: The data containing non-indexed log parameter.
- ``raw.topics`` - ``Array``: An array with max 4 32 Byte topics, topic 1-3 contains indexed parameters of the event.

-------
Example
-------

.. code-block:: javascript

    myContract.events.MyEvent({
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
        fromBlock: 0
    }, function(error, event){ console.log(event); })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on('data', function(event){
        console.log(event); // same results as the optional callback above
    })
    .on('changed', function(event){
        // remove event from local database
    })
    .on('error', function(error, receipt) { // If the transaction was rejected by the network with a receipt, the second parameter will be the receipt.
        ...
    });

    // event output example
    > {
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    }


------------------------------------------------------------------------------

events.allEvents
=====================

.. code-block:: javascript

    myContract.events.allEvents([options][, callback])

Same as :ref:`events <contract-events>` but receives all events from this smart contract.
Optionally the filter property can filter those events.


------------------------------------------------------------------------------


getPastEvents
=====================

.. code-block:: javascript

    myContract.getPastEvents(event[, options][, callback])

Gets past events for this contract.

----------
Parameters
----------

1. ``event`` - ``String``: The name of the event in the contract, or ``"allEvents"`` to get all events.
2. ``options`` - ``Object`` (optional): The options used for deployment.
    * ``filter`` - ``Object`` (optional): Lets you filter events by indexed parameters, e.g. ``{filter: {myNumber: [12,13]}}`` means all events where "myNumber" is 12 or 13.
    * ``fromBlock`` - ``Number|String|BN|BigNumber`` (optional): The block number (greater than or equal to) from which to get events on. Pre-defined block numbers as ``"latest"``, ``"earlist"``, ``"pending"``, and ``"genesis"`` can also be used.
    * ``toBlock`` - ``Number|String|BN|BigNumber`` (optional): The block number (less than or equal to) to get events up to (Defaults to ``"latest"``). Pre-defined block numbers as ``"latest"``, ``"earlist"``, ``"pending"``, and ``"genesis"`` can also be used.
    * ``topics`` - ``Array`` (optional): This allows manually setting the topics for the event filter. If given the filter property and event signature, (topic[0]) will not be set automatically.
3. ``callback`` - ``Function`` (optional): This callback will be fired with an array of event logs as the second argument, or an error as the first argument.


-------
Returns
-------

``Promise`` returns ``Array``: An array with the past event ``Objects``, matching the given event name and filter.

For the structure of a returned event ``Object`` see :ref:`getPastEvents return values <contract-events-return>`.

-------
Example
-------

.. code-block:: javascript

    myContract.getPastEvents('MyEvent', {
        filter: {myIndexedParam: [20,23], myOtherIndexedParam: '0x123456789...'}, // Using an array means OR: e.g. 20 or 23
        fromBlock: 0,
        toBlock: 'latest'
    }, function(error, events){ console.log(events); })
    .then(function(events){
        console.log(events) // same results as the optional callback above
    });

    > [{
        returnValues: {
            myIndexedParam: 20,
            myOtherIndexedParam: '0x123456789...',
            myNonIndexParam: 'My String'
        },
        raw: {
            data: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
            topics: ['0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7', '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385']
        },
        event: 'MyEvent',
        signature: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        logIndex: 0,
        transactionIndex: 0,
        transactionHash: '0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385',
        blockHash: '0xfd43ade1c09fade1c0d57a7af66ab4ead7c2c2eb7b11a91ffdd57a7af66ab4ead7',
        blockNumber: 1234,
        address: '0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe'
    },{
        ...
    }]

