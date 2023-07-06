- **parse**
	- ```
	  parsers: # array
	    # - reg: ^.*$ 匹配所有订阅，或 - url: https://example.com/profile.yaml 指定订阅
	    - reg: ^.*$
	      # 删除服务商提供的策略组和规则
	      code: |
	        module.exports.parse = (raw, { yaml }) => {
	          const rawObj = yaml.parse(raw)
	          const groups = []
	          const rules = []
	          return yaml.stringify({ ...rawObj, 'proxy-groups': groups, rules })
	        }
	      # 建立自己的配置
	      yaml:
	        # 建立策略组
	        prepend-proxy-groups:
	          - name: 手动 # 手动选择代理
	            type: select
	  
	          - name: Netflix # Netflix
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Hulu # Hulu
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Disney # Disney
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: HBO # HBO
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Prime Video # Prime Video
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: YouTube # YouTube
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Spotify # Spotify
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: iQIYI # iQIYI
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Bilibili # Bilibili
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: PayPal # PayPal
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: Stripe # Stripe
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: OpenAI # OpenAI
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: Google Bard # Google Bard
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: new Bing # new Bing
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: Claude # Claude
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: Poe # Poe
	            type: select
	            proxies:
	              - US
	              - JP
	              - SG
	              - TW
	              - HK
	              - 手动
	              - DIRECT
	  
	          - name: Games CN # Games CN
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Games # Games
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Google FCM # Google FCM
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: OneDrive # OneDrive
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Microsoft CN # Microsoft CN
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Microsoft # Microsoft
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: iCloud CN # iCloud CN
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: iCloud # iCloud
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Apple CN # Apple CN
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Apple # Apple
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Social Media # Social Media
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Scholar CN # Scholar CN
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Scholar # Scholar
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: Global Direct # Global Direct
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: CN Direct # CN Direct
	            type: select
	            proxies:
	              - DIRECT
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	  
	          - name: Final # Final
	            type: select
	            proxies:
	              - HK
	              - TW
	              - SG
	              - JP
	              - US
	              - 手动
	              - DIRECT
	  
	          - name: HK # 自动选择延迟最低的香港节点
	            type: url-test
	            url: http://www.gstatic.com/generate_204
	            interval: 300
	            tolerance: 50
	            lazy: true
	  
	          - name: TW # 自动选择延迟最低的台湾节点
	            type: url-test
	            url: http://www.gstatic.com/generate_204
	            interval: 300
	            tolerance: 50
	            lazy: true
	  
	          - name: SG # 自动选择延迟最低的新加坡节点
	            type: url-test
	            url: http://www.gstatic.com/generate_204
	            interval: 300
	            tolerance: 50
	            lazy: true
	  
	          - name: JP # 自动选择延迟最低的日本节点
	            type: url-test
	            url: http://www.gstatic.com/generate_204
	            interval: 300
	            tolerance: 50
	            lazy: true
	  
	          - name: US # 自动选择延迟最低的美国节点
	            type: url-test
	            url: http://www.gstatic.com/generate_204
	            interval: 300
	            tolerance: 50
	            lazy: true
	  
	          # 策略组示例
	          # - name: 分组名
	          #   type: select       # 手动选点
	          #         url-test     # 自动选择延迟最低的节点
	          #         fallback     # 节点故障时自动切换下一个
	          #         laod-balance # 均衡使用分组内的节点
	          #   url: http://www.gstatic.com/generate_204 # 测试地址 非 select 类型分组必要
	          #   interval: 300 # 自动测试间隔时间，单位秒 非 select 类型分组必要
	          #   tolerance: 50 # 允许的偏差，节点之间延迟差小于该值不切换 非必要
	          #   proxies:
	          #     - 节点名称或其他分组套娃
	  
	        commands:
	          - proxy-groups.HK.proxies=[]proxyNames|^(.*)(HK|香港|港|Hong Kong)+(.*)$ # 向指定策略组添加订阅中的节点名，可使用正则过滤
	          - proxy-groups.TW.proxies=[]proxyNames|^(.*)(TW|台湾|台|Taiwan)+(.*)$
	          - proxy-groups.SG.proxies=[]proxyNames|^(.*)(SG|新加坡|坡|Singapore)+(.*)$
	          - proxy-groups.JP.proxies=[]proxyNames|^(.*)(JP|日本|日|Japan)+(.*)$
	          - proxy-groups.US.proxies=[]proxyNames|^(.*)(US|美国|美|United States)+(.*)$
	          - proxy-groups.手动.proxies=[]proxyNames
	          # - proxy-groups.手动.proxies.0+HK
	          # - proxy-groups.手动.proxies.1+TW
	          # - proxy-groups.手动.proxies.2+SG
	          # - proxy-groups.手动.proxies.3+JP
	          # - proxy-groups.手动.proxies.4+US
	  
	          # 一些可能用到的正则过滤节点示例，使分组更细致
	          # []proxyNames|a                         # 包含 a
	          # []proxyNames|^(.*)(a|b)+(.*)$          # 包含 a 或 b
	          # []proxyNames|^(?=.*a)(?=.*b).*$        # 包含 a 和 b
	          # []proxyNames|^((?!b).)*a((?!b).)*$     # 包含 a 且不包含 b
	          # []proxyNames|^((?!b|c).)*a((?!b|c).)*$ # 包含 a 且不包含 b 或 c
	  
	        # 添加规则
	        prepend-rules: # 规则由上往下遍历，如上面规则已经命中，则不再往下处理
	          - RULE-SET,netflix,Netflix
	          - RULE-SET,hulu,Hulu
	          - RULE-SET,disney,Disney
	          - RULE-SET,hbo,HBO
	          - RULE-SET,primevideo,Prime Video
	          - RULE-SET,youtube,YouTube
	          - RULE-SET,spotify,Spotify
	          - RULE-SET,iqiyi,iQIYI
	          - RULE-SET,bilibili,Bilibili
	          - RULE-SET,paypal,PayPal
	          - RULE-SET,stripe,Stripe
	          - RULE-SET,openai,OpenAI
	          - RULE-SET,google,Google Bard
	          - RULE-SET,bing,new Bing
	          - RULE-SET,anthropic,Claude
	          - RULE-SET,quora,Poe
	          - RULE-SET,games@cn,Games CN
	          - RULE-SET,games,Games
	          - RULE-SET,googlefcm,Google FCM
	          - RULE-SET,onedrive,OneDrive
	          - RULE-SET,microsoft@cn,Microsoft CN
	          - RULE-SET,microsoft,Microsoft
	          - RULE-SET,icloud@cn,iCloud CN
	          - RULE-SET,icloud,iCloud
	          - RULE-SET,apple@cn,Apple CN
	          - RULE-SET,apple,Apple
	          - RULE-SET,telegram,Social Media
	          - RULE-SET,discord,Social Media
	          - RULE-SET,slack,Social Media
	          - RULE-SET,zoom,Social Media
	          - RULE-SET,scholar@cn,Scholar CN
	          - RULE-SET,scholar,Scholar
	          # - DOMAIN,clash.razord.top,DIRECT
	          # - DOMAIN,yacd.haishan.me,DIRECT
	          - GEOIP,LAN,Global Direct
	          - GEOIP,CN,CN Direct
	          - MATCH,Final
	  
	        # 添加规则集
	        mix-rule-providers:
	          openai: # OpenAI
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/openai.yaml"
	            path: ./ruleset/openai.yaml
	            interval: 86400
	  
	          google: # Google Bard
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/google.yaml"
	            path: ./ruleset/google.yaml
	            interval: 86400
	  
	          bing: # new Bing
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/bing.yaml"
	            path: ./ruleset/bing.yaml
	            interval: 86400
	  
	          netflix: # Netflix
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/netflix.yaml"
	            path: ./ruleset/netflix.yaml
	            interval: 86400
	  
	          hulu: # Hulu
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/hulu.yaml"
	            path: ./ruleset/hulu.yaml
	            interval: 86400
	  
	          disney: # Disney
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/disney.yaml"
	            path: ./ruleset/disney.yaml
	            interval: 86400
	  
	          hbo: # HBO
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/hbo.yaml"
	            path: ./ruleset/hbo.yaml
	            interval: 86400
	  
	          primevideo: # Prime Video
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/primevideo.yaml"
	            path: ./ruleset/primevideo.yaml
	            interval: 86400
	  
	          youtube: # YouTube
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/youtube.yaml"
	            path: ./ruleset/youtube.yaml
	            interval: 86400
	  
	          spotify: # Spotify
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/spotify.yaml"
	            path: ./ruleset/spotify.yaml
	            interval: 86400
	  
	          iqiyi: # iQIYI
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/iqiyi.yaml"
	            path: ./ruleset/iqiyi.yaml
	            interval: 86400
	  
	          bilibili: # Bilibili
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/bilibili.yaml"
	            path: ./ruleset/bilibili.yaml
	            interval: 86400
	  
	          games@cn: # Games CN
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/category-games@cn.yaml"
	            path: ./ruleset/games@cn.yaml
	            interval: 86400
	  
	          games: # Games
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/category-games.yaml"
	            path: ./ruleset/games.yaml
	            interval: 86400
	  
	          googlefcm: # Google FCM
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/googlefcm.yaml"
	            path: ./ruleset/googlefcm.yaml
	            interval: 86400
	  
	          onedrive: # OneDrive
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/onedrive.yaml"
	            path: ./ruleset/onedrive.yaml
	            interval: 86400
	  
	          microsoft@cn: # Microsoft CN
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/microsoft@cn.yaml"
	            path: ./ruleset/microsoft@cn.yaml
	            interval: 86400
	  
	          microsoft: # Microsoft
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/microsoft.yaml"
	            path: ./ruleset/microsoft.yaml
	            interval: 86400
	  
	          icloud@cn: # iCloud CN
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/icloud@cn.yaml"
	            path: ./ruleset/icloud@cn.yaml
	            interval: 86400
	  
	          icloud: # iCloud
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/icloud.yaml"
	            path: ./ruleset/icloud.yaml
	            interval: 86400
	  
	          apple@cn: # Apple CN
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/apple@cn.yaml"
	            path: ./ruleset/apple@cn.yaml
	            interval: 86400
	  
	          apple: # Apple
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/apple.yaml"
	            path: ./ruleset/apple.yaml
	            interval: 86400
	  
	          telegram: # Telegram
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/telegram.yaml"
	            path: ./ruleset/telegram.yaml
	            interval: 86400
	  
	          discord: # Discord
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/discord.yaml"
	            path: ./ruleset/discord.yaml
	            interval: 86400
	  
	          slack: # Slack
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/slack.yaml"
	            path: ./ruleset/slack.yaml
	            interval: 86400
	  
	          zoom: # Zoom
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/zoom.yaml"
	            path: ./ruleset/zoom.yaml
	            interval: 86400
	  
	          scholar@cn: # Scholar CN
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/category-scholar-cn.yaml"
	            path: ./ruleset/scholar@cn.yaml
	            interval: 86400
	  
	          scholar: # Scholar
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/category-scholar-%21cn.yaml"
	            path: ./ruleset/scholar.yaml
	            interval: 86400
	  
	          paypal: # PayPal
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/paypal.yaml"
	            path: ./ruleset/paypal.yaml
	            interval: 86400
	  
	          stripe: # Stripe
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/stripe.yaml"
	            path: ./ruleset/stripe.yaml
	            interval: 86400
	  
	          anthropic: # Claude
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/anthropic.yaml"
	            path: ./ruleset/anthropic.yaml
	            interval: 86400
	  
	          quora: # Poe
	            type: http
	            behavior: domain
	            url: "https://rules.kr328.app/quora.yaml"
	            path: ./ruleset/quora.yaml
	            interval: 86400
	  ```