---
description: >-
  Lightning Terminal bundles Loop to make it easy for you to manage your channel
  liquidity.
---

# Loop and Lightning Terminal

Lightning Terminal offers a graphical interface for Loop, making it easy and intuitive to make submarine swaps. [Lightning Loop](../loop/) is a service that allows users to make a Lightning transaction to an on-chain Bitcoin address (Loop Out), or send on-chain Bitcoin directly into a Lightning channel (Loop In).

Loop can help manage channel liquidity, for example, by emptying out a channel and [acquiring inbound capacity](../../the-lightning-network/liquidity/how-to-get-inbound-capacity-on-the-lightning-network.md) (or refilling a depleted channel).

These actions can be automated with Autoloop.

[Learn more about how Submarine Swaps work.](../../the-lightning-network/multihop-payments/understanding-submarine-swaps.md)

## How to use Loop in Lightning Terminal <a href="#docs-internal-guid-eae8e6fb-7fff-9fc5-7155-0aae66bbe668" id="docs-internal-guid-eae8e6fb-7fff-9fc5-7155-0aae66bbe668"></a>

On the top side of Lightning Terminal you see ‘Loop.’ Clicking on it shows an overview over your channels and their balances.&#x20;

To perform a Loop, you can slide the bar to the left, meaning you decrease your Lightning balance and receive onchain funds in return, called a Loop Out. You can also perform a Loop In, which refills your Lightning channels using your onchain balance. You can also select individual channels that you want to empty or refill.

Use the slider to choose how many satoshis you want to swap. A warning will appear if you do not meet the minimum swap size.

### Loop

If you would like to perform this swap once, choose _Loop._ You'll see a summary of your order, including a breakdown of the fees, including the expected onchain fee and the Loop fee. For Loop Outs, a prepayment of 30,000 satoshis is required.

In the additional options, you can choose your confirmation speed (as measured in blocks, more blocks meaning lower fees). If your goal is to send funds into cold storage or an external wallet, enter your address here.

Upon clicking “Submit”, your Loop is submitted.

#### Loop In

If you perform a Loop In, the Loop server will probe whether it is able to make an off-chain payment of the selected size to your node. If such a probe is successful, Lightning Terminal will instruct your node to send on-chain funds to the swap address. Once this payment is confirmed, the off-chain payment is made to your node. From your perspective the Loop In is now complete.

#### Loop Out

If you perform a Loop Out, your node will probe whether it can reach the Loop node with a payment of the chosen size. If it can reach it, the Loop server will send the funds to a 2-of-2 multisignature contract.

Once confirmed, Lightning Terminal will instruct your node to make the off-chain payment. As soon as the payment succeeds and Terminal has obtained the preimage, your node will automatically sweep the funds from the multisignature contract and have them on its disposal.

In your Dashboard, you should now be able to see three new transactions. One, over 30,000 satoshis is the prepayment, which will be forfeited if the off-chain transaction is not being made, another over the full amount and a third on-chain transaction sending the funds to your wallet. If you specified an external address, this third transaction will be visible in the corresponding wallet.

[Learn more about Loop fees](loop-fees.md).

#### Loop failures <a href="#docs-internal-guid-322553ac-7fff-f559-9670-7d14f9cf1697" id="docs-internal-guid-322553ac-7fff-f559-9670-7d14f9cf1697"></a>

Loops primarily fail because of missing liquidity between you and the Loop node. For example, the peer whose channel you want to Loop Out of might not have enough outgoing capacity to Loop themselves. If you experience a failure, you may try Loop In or Out with a different channel, or lower the amount of your swap.

### Loop status <a href="#docs-internal-guid-386c8c7b-7fff-759e-997e-a636a509508e" id="docs-internal-guid-386c8c7b-7fff-759e-997e-a636a509508e"></a>

The process of your Loop In or Loop Out is structured into three different steps:

#### Initiated

When initiating a Loop, your node or Loop verifies that it has the ability to make an off-chain payment to the other side. If a swap fails, it is most commonly at this stage, due to lack of liquidity along the route. You may try again with a different channel or a smaller amount. If your swap fails at this stage, you are not charged any fees.

#### Preimage revealed

Once a path has been found for the off-chain funds, the on-chain transaction is made to the submarine swap contract. Once it is confirmed, the recipient of the off-chain payment reveals the preimage and claims the off-chain funds irreversibly for themselves. This in return allows the recipient to claim the on-chain funds from the swap contract. If your Loop fails at this stage, the sender of the on-chain funds will have to sweep the funds back to themselves. If you are performing a Loop In, this will cost you in transaction fees, if you are performing a Loop Out, you are charged a fee by Loop through a Lightning payment. This most commonly happens if the user goes offline or turns off litd or loopd during the swap.

#### Success

Once both parties have received their funds, the Loop is complete.

### Autoloop

Autoloops can be configured for specific peers or your node as a whole. After moving the slider at the top, select Autoloop to configure your recurring swap. You may also move the slider on a specific channel.

You can set a minimum Loop size. As a big portion of fees are onchain swaps, a higher minimum size can help making swaps more economical.

You can also control costs by setting a maximum fee per swap, measured in BPS (1 BPS = 100 PPM = 0.01%). During times of high onchain fees, Terminal might not initiate Loops, or perform larger Loops. You can also choose a maximum budget per day, week or month.

It is also possible to send Loop Outs to an external address, such as cold storage.

Finally, you'll get a chance to review your Autoloop.

Once Autoloop has been configured, you should be able to see previous Loops and the current status under the channel for which it has been activated. Here you can also remove the Autoloop rule.



<figure><img src="../../.gitbook/assets/Screenshot from 2025-01-17 17-45-19.png" alt=""><figcaption><p>Configure Autoloop Settings in Lightning Terminal</p></figcaption></figure>
