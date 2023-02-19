---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/teaching.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../../lectures/css/datavis.less"

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

<!-- Everything above this line should probably be left untouched. -->

# Session 3: Practical Exercises

{(task|}

Use this document as a place to add your answers to the week's practical exercises.

{|task)}

### 1. Measurement Types and Colour Schemes

![Good discussion topic](https://img.shields.io/badge/Good%20discussion%20topic-blue.svg)

Which measurement types (e.g. `Nominal`, `Ordinal`, `Quant`, `Temporal`) and which [colour schemes](https://vega.github.io/vega/docs/schemes/) (e.g. `category20`, `purples` etc.) would you specify for the following data. Briefly add your reasoning to the table below and then copy it to your exercises document in your coursework portfolio repo:

| Data                                                                                           | Measurement type | Colour Scheme | Justification                                                                          |
| ---------------------------------------------------------------------------------------------- | ---------------- | ------------- | -------------------------------------------------------------------------------------- |
| 1. Student cohorts in an attendance streamgraph                                                | Ordinal          | purples       | it's a count graph so has to be ordinal                                                |
| 2. FTSE 100 indices over the last 12 months                                                    | Temporal         | green-red     | it's over a time period and will use green for increases and red for dips              |
| 3. Your assessment marks for all completed modules                                             | Quant            | black and red | it's based on discrete numbers and red will be used for fails and black are for passes |
| 4. Political party of elected MPs                                                              | Nominal          | purple gold   | it's like a name or label so there's no ordering to it                                 |
| 5. Responses to a 5-point [Likert](https://en.wikipedia.org/wiki/Likert_scale) survey question | Ordinal          | blues         | the responses will have a count so colour hues will be appropriate                     |

### 2. Modifying the attendance streamgraph

Modify the attendance streamgraph provided near the top of these lecture notes so that it uses the colour scheme you've recommended in the question above. If your recommendation was the existing scheme, see if you can find a better scheme and briefly say how it is an improvement.

```elm {v l}
attendance : Spec
attendance =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration (coAxis [ axcoTicks False, axcoDomain False, axcoLabelAngle 0 ])

        data =
            dataFromUrl "https://gicentre.github.io/data/attendance.csv"
                -- Force session values to be treated as numbers not text
                [ parse [ ( "session", foNum ) ] ]

        enc =
            encoding
                << position X [ pName "session" ]
                << position Y
                    [ pName "attendance"
                    , pQuant
                    , pStack stCenter -- Stacked from the centre not bottom.
                    , pAxis []
                    ]
                << detail [ dName "id" ]
                << color [ mName "cohort", mScale [ scScheme "purples" [] ] ]
    in
    toVegaLite
        [ width 600
        , height 300
        , cfg []
        , data
        , enc []
        , area
            [ maLine (lmMarker []) -- Add lines around each area 'stream'
            , maInterpolate miMonotone -- Monotone interpolation gives curved lines
            ]
        ]
```

### 3. Global Temperature Data Challenge

The Economist famously used colour in its data visualization of global temperature anomalies on its [2019 cover](https://www.economist.com/leaders/2019/09/19/the-climate-issue?fsrc=scn/tw/te/bl/ed/theclimateissueawarmingworld). That is, the difference in mean global temperature compared to the 1950-1980 average.

![Economist climate issue cover](https://staff.city.ac.uk/~jwo/datavis2023/session03/images/economistCover.jpg)

How has colour been used here, and how effective do you think the design is? Are there any problems with this colour mapping?

Using a more [detailed version of the data](https://datahub.io/core/global-temp) containing monthly temperature anomaly values, here is a simple line chart.

```elm {l v}
linechart : Spec
linechart =
    let
        data =
            dataFromUrl "https://gicentre.github.io/data/temperatureAnomalies.json" []

        enc =
            encoding
                << position X [ pName "Date", pTemporal ]
                << position Y [ pName "Anomaly", pQuant ]
                << color [ mName "Anomaly", mQuant, mScale
                        [ scScheme "redblue" []
                        , scDomain (doMid 0.15)
                        ]
                       ]
    in
    toVegaLite [ width 600, data, bar [], enc [] ]
```

Modify the chart to make use of colour to emphasise the trends over time and possible seasonal cycles (the data are provided monthly since 1900). Consider the effect of changing the [line](https://package.elm-lang.org/packages/gicentre/elm-vegalite/latest/VegaLite#line) mark to a [bar](https://package.elm-lang.org/packages/gicentre/elm-vegalite/latest/VegaLite#bar) or other type of mark when exploring design possibilities.

Briefly add some text to your litvis document justifying your colour design choices.

JUSTIFICATION:
As the anomalies range between a +1 and -0.5, I added a diverging colour scheme and added a domain midpoint of 0.15. The hues of the scheme make it really easy to know which years had the highest changes.

### 4. Design Challenge

![Good discussion topic](https://img.shields.io/badge/Good%20discussion%20topic-blue.svg)

One way of developing your design skills is deliberately to constrain your design choices and see if you can come up with creative solutions under those constraints. Using this idea of [constrained creativity](https://www.fastcompany.com/3067925/how-constraints-force-your-brain-to-be-more-creative), try to come up with a design for showing gender and name frequency over time (as shown in [Baby Name Voyager](https://web.archive.org/web/20211015175350/https://www.babynamewizard.com/namevoyager-expert#prefix=&sw=both&exact=false) at the start of this lecture) that does not use colour (i.e. use only black or white in your design). For inspiration you may wish to have a look at the finalists in this [MonoCarto competition](https://somethingaboutmaps.wordpress.com/monocarto-2019-winners/).

Sketch out your design and include a photo of it in your `practicalExercises.md` portfolio document. As a reminder, you can include an image in a litvis document like this:

```markdown
![image caption](myImagefile.jpg)
```

---

_Check your progress._

- [ ] I can specify colours explicitly using decimal and hex RGB triplets.
- [ ] I can specify colours using HSL triplets.
- [ ] I can specify colours using explicitly named colours.
- [ ] I can identify _nominal_, _ordinal_, _quantitative_ and _temporal_ measurement types.
- [ ] I can match measurement type to an appropriate colour scheme.
- [ ] I can specify colour encodings that accommodate the most common forms of colour vision deficiencies.
