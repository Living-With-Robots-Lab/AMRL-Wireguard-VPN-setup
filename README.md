# AMRL-Wireguard-VPN-setup

## Linux Machine

1. Install wireguard:
    ```
    sudo apt-get install wireguard wireguard-tools
    ```
2. Create a private / public key pair.
    ```
    cd /etc/wireguard
    wg genkey | tee wg-private.key | wg pubkey > wg-public.key
    ```
3. Send your wg public key and UT EID to Nikunj or someone from the AMRL lab (Amanda or Arthur) and get your assigned IP address.  
4. Create wireguard config file `/etc/wireguard/wg0.conf ` on your computer:
    **IMPORTANT: DO NOT CHANGE THE CONTENTS UNDER [Peer], only edit the details under [Interface] with your assigned address and your private key**
    ```
    [Interface]
    Address = <Assigned IP Address by AMRL> # e.g. 10.0.0.101/32
    PrivateKey = <WG Private Key>
    [Peer]
    PublicKey = AANZBeRHBxUGO1W6Flq63ThnlJlxT93XNuGtqiwPz1w=
    AllowedIPs = 10.0.0.0/24,10.1.0.0/16,10.2.0.0/16,10.3.0.0/16 # routes all traffic from 10.0.0.[0-255] and 10.[1-3].[0-255].[0-255].
    # Use DNS host lookups for regular clients:
    Endpoint = robofleet.csres.utexas.edu:51820
    # Use the following line instead of the above on robots, to bypass host lookups:
    # Endpoint = 128.83.143.97:51820
    PersistentKeepalive = 10
    ```
5. Start the wireguard interface with `sudo wg-quick up wg0`, or `sudo wg-quick up FULL_PATH_TO_CONFIG`.
6. To set wireguard to load on boot:
    ```
    sudo systemctl enable wg-quick@wg0
    ```

## Mac Machine
1. Install wireguard:
    - Go to [website for wireguard app](https://apps.apple.com/us/app/wireguard/id1451685025?mt=12) and click “view in Mac App Store”
    - Once open in Mac store, Get, then Install
2. Create a wireguard tunnel
    - In the wireguard application, click the plus, and select "Add empty tunnel".
    - Using the private key that is automatically populated in the text box for \<Private Key\>, paste in the wireguard configuration found in step 4 of "Linux Machine".
    - The IP address field should be a unique (and not already used) IP address that is assigned to your computer. Get yout assigned ip by following step 3 of "Linux Machine"
3. (When using) Select your configured tunnel and click "Activate"


## How to Connect to BWIBOTs?
- use ``` ssh {username}@{IP ADDRESS OF BWIBOT} ```
- Example: For Dobby use: ssh bwilab@10.0.0.140
