# NFTMysteryBox

Mystery Box 中文通常現在叫他[盲盒], 指的是在發行團隊指定的條件(時間,銷售數量等)達到前, 買家所看到的NFT圖案統一為預設圖片，待解盲後才會看到獲得的隨機NFT圖片。   
這種商業上的玩法也是如今NFT銷售的很大一個主流, 這邊就來實作一下簡單版的盲盒智能合約該如何製作.  

1. 提供解盲前後的不同URI用以連接到不同的MetaData, 來顯示盲盒->真正的NFT   
2. 提供setMysteryBoxOpen來模擬項目方進行整批解盲的動作   

提供解盲前後Bool判斷以及各自的tokenURI   
```Solidity
// Default set Box is not opened
    bool private _blindBoxOpened = false;
    string private _blindTokenURI = "https://gateway.pinata.cloud/ipfs/QmRx7xyp6o6Rc9XvQiMRjjJH8K9BAssxv8kvXEjqdaZi2z/";
    string private _openTokenURI = "https://gateway.pinata.cloud/ipfs/QmRoENYamuX58oHPRaCeAsPPxY5Mdk5NS4dwEkTbJoo8Hj/";
```

以BaseURI來進行顯示的控制, 並提供一個查詢目前是否為解盲階段的功能   
```Solidity
    function setMysteryBoxOpen() external{
        _blindBoxOpened = true;
        baseURI_ = _openTokenURI;
    }

    function setMysteryBoxClose() external{
        _blindBoxOpened = false;
        baseURI_ = _blindTokenURI;
    }

    function _isBlindBoxOpened() internal view returns (bool) {
        return _blindBoxOpened;
    }
```
部署後可以在opensea看到解盲前的顯示狀態為顯示可愛的怪獸盒子。  
<img width="1458" alt="image" src="https://user-images.githubusercontent.com/24216536/197561721-dffb8249-6291-4d0a-bd37-35468c05642f.png">   

可以先在Remix看到tokenURI已經改為QmRo打頭的openTokenURI   
<img width="341" alt="image" src="https://user-images.githubusercontent.com/24216536/197562503-c88a07dd-a76c-42d5-8b09-3c328ccd213d.png">.  
opensea更新之後~~~就出現可愛的怪獸圖片啦~撒花   
<img width="1175" alt="image" src="https://user-images.githubusercontent.com/24216536/197562844-38f3ab2b-e3b7-4162-891f-00dc60e36b05.png">   

當然也可以再將tokenID帶入開關的公式, 就可以保留開盲盒的權力給個人地址嚕!  

但盲盒仍要注意的部分有兩個情境.  
1. 項目方是否可以預先控制mint token id,是否可以人工控制mint出的項目.  
2. 擁有者後續是否可以驗證其token id是否有被調整,篡改過.  

避開上述黑箱作業的項目, 才可以體驗到一個真正隨機的盲盒NFT開箱經驗!~~.  

