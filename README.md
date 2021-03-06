# OpenBullet2Python
OpenBullet2Python using code from OpenBullet

This is not a user enumeration tool. The name is borrowed from OpenBullet but will not have support for wordlists, wordlist types.

# Parsing Loliscript
- [x] PARSE
- [x] REQUEST
- [x] KEYCHECK
- [x] FUNCTIONS


```Python
from OpenBullet2Python.ToPython import ToPython
config_text = """REQUEST GET "https://google.com" 
  
  HEADER "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36" 
  HEADER "Pragma: no-cache" 
  HEADER "Accept: */*" """
  
blocks = ToPython(config_text)
```

```
>>> [{'Method': 'GET', 'Url': 'https://google.com', 'Booleans': {}, 'CustomCookies': {}, 'CustomHeaders': {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36', 'Pragma': 'no-cache', 'Accept': '*/*'}, 'label': '', 'block_type': 'REQUEST'}]
```

# Processing Blocks
- FUNCTION
  - [x] Constant
  - [x] Base64Encode
  - [x] Base64Decode
  - [x] ToUppercase
  - [x] ToLowercase
  - [x] Length
  - [x] Replace
    - [ ] UseRegex
  - [x] URLEncode
  - [x] URLDecode
  - [x] Hash
    - [ ] MD4
  - [x] HMAC
    - [ ] MD4
  - [x] RandomNum
  - [x] RandomString
- REQUEST
  - [x] Standard
    - [x] AutoRedirect
    - [x] EncodeContent
  - [x] BasicAuth # Not tested
  - [ ] Multipart
  - [ ] Raw
- PARSE
  - [x] LR
    - [x] UseRegexLR
  - [ ] CSS
  - [x] JSON
    - [x] JToken
  - [x] REGEX
    - [ ] EncodeOutput
    - [ ] DotMatches
- KEYCHECK
  - [ ] CUSTOM 
  
 
```Python
from OpenBullet2Python.Models.BotData import BotData
from OpenBullet2Python.TestConfig import TestConfig
from OpenBullet2Python.Models.CVar import CVar

# This holds the variable list.
data = BotData()

# By default Status is set to BotStatus.NONE
print(data.Status.value)

# Make sure it is a raw string or python will encode \" as "
config_text = r"""REQUEST GET "https://google.com" 
  
  HEADER "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36" 
  HEADER "Pragma: no-cache" 
  HEADER "Accept: */*" 

KEYCHECK 
  KEYCHAIN Success OR 
    KEY "title>Google" """

# Adding a variable for replacement
# This will add a variable called "USER" with the value as "username", ect
data.Variables.Set(CVar("USER","username",False,True))
data.Variables.Set(CVar("PASS","password",False,True))

# Run the config
# If the the status changes to Fail, Ban or an Error then it will return, else it runs until all the blocks are processed.
# There is no error handling so be careful on what you run.
TestConfig(config_text,data)

# The outcome of the config test
print(data.Status.value)
```
```
>>> NONE
>>> Calling https://google.com
>>> SUCCESS
```
 https://github.com/openbullet/openbullet
