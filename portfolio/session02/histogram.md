---
follows: imdbRoot

import Elemental
import Elemental.Encoding as Encoding
import Elemental.Encodable as Encodable
import VegaLite.Axis (HAlign)
---

A default histogram:

```elm {l}
goldenRatio : Float
goldenRatio =
    1.618
```

```elm {l v}

histogram : Spec
histogram =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis
                        [ axcoTicks False
                        , axcoDomain False
                        , axcoLabelAngle 0
                        , axcoLabelPadding -12
                        ]
                    )

        enc =
            encoding
                << position X [ pName "IMDB Rating", pBin [],pScale [ scPaddingInner 0.5], pAxis [ axTitle "" ] ]
                << position Y [ pAggregate opCount, pAxis [ axTitle "Count" ] ]
                << color [ mName "IMDB Rating", mOrdinal]

    in
    toVegaLite [ data, cfg[], enc [], bar [maFillOpacity 0.8], height (450 / goldenRatio)]
```

I tried changing the colours but it kept giving me a bunch of weird erros and my visual just became grey. I reduced the opacity of the bars and removed the labels on the x-axis, reducing the data ink.
