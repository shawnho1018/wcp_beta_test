# wcp_beta_test
WCP目前支援內置與外接NSX-T的設定。目前PM所提供的[預設文件](https://communities.vmware.com/docs/DOC-40827)是內接的，但我們更建議使用外接的NSX-T進行設定，[外接的配置文件可由此取得](https://confluence.eng.vmware.com/pages/viewpage.action?pageId=543354268)。外部配置有幾個好處：
1. NSX-T可作為全局的使用，不僅限於WCP (Project Pacific)的使用。 
2. 安裝經驗與PKS的安裝類似，僅在Overlay的部分有微調。
