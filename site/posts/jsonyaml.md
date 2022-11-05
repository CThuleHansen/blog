---
title: "JSON YAML Parsing - Just why?"
author: Casper Thule
date: Nov 5, 2022
tags: [Haskell]
description: Trying JSON/YAML parsing in Haskell
---

I might have touched upon one of the Haskell things that seem unnecessarily complex - apparently JSON and YAML decoding has to quite different type signatures.

For JSON parsing I am using aeson (https://hackage.haskell.org/package/aeson), more specifically this function: 

```eitherDecode :: FromJSON a => ByteString -> Either String a```

For YAML parsing I am using yaml (https://hackage.haskell.org/package/yaml), more specifically this function:

```decodeEither' :: FromJSON a => ByteString -> Either ParseException a```

As it can be seen from the type signatures above, it is necessary to perform some handling of the `Left` case due to the difference in String vs ParseException. Furthermore, the `ByteString` also differ between the two. Aeson uses `Data.ByteString.Lazy` whereas yaml uses `Data.ByteString.Lazy`. This can easily be fixed though, by using 
`Data.Aeson.eitherDecodeStrict` instead. 

Thus, I ended up with a wrapper:
```
yamlDecode :: FromJSON a => Data.ByteString.ByteString ->  Either String a
yamlDecode a = case Data.Yaml.decodeEither' a of  
  Left pe -> Left $ show pe
  Right a' -> Right a'
```

Cases such as this one are exactly the reason why I am creating these sort of templates - to familiarize myself with the ecosystem.
