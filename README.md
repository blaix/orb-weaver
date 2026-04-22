# Orb Weaver

A framework for creating mobile and desktop applications in [Gren](https://gren-lang.org/).

Still in very early development.

## Example

Classic counter example, as a cross-platform mobile/desktop app:

```elm
module Main exposing (main)

import Orb
import Orb.Platform exposing (Desktop, Mobile)
import UI
import UI.Attribute as Attr
import UI.Desktop
import UI.Mobile


main : Orb.Program {} Model Msg
main =
    Orb.program
        { init = init
        , update = update
        , subscriptions = subscriptions
        , mobileView = mobileView
        , desktopView = desktopView
        }


type alias Model =
    { count : Int
    }


init : {} -> { model : Model, command : Cmd Msg }
init _ =
    { model = { count = 0 }
    , command = Cmd.none
    }


type Msg
    = Increment
    | Decrement


view : Model -> UI.Element platform Msg
view model =
    UI.column [ Attr.padding 16 ]
        [ UI.text [ Attr.textStyle Attr.HeroLarge ] (String.fromInt model.count)
        , UI.row []
            [ UI.button [] { onPress = Decrement, label = UI.text [] "-" }
            , UI.button [] { onPress = Increment, label = UI.text [] "+" }
            ]
        ]


mobileView : Model -> UI.Element Mobile Msg
mobileView model =
    UI.Mobile.navigationStack { title = "My Mobile Counter" }
        (view model)


desktopView : Model -> UI.Element Desktop Msg
desktopView model =
    UI.Desktop.window { title = "My Desktop Counter" }
        (view model)


update : Msg -> Model -> { model : Model, command : Cmd Msg }
update msg model =
    when msg is
        Increment ->
            { model = { model | count = model.count + 1 }
            , command = Cmd.none
            }

        Decrement ->
            { model = { model | count = model.count - 1 }
            , command = Cmd.none
            }


subscriptions : Model -> Sub Msg
subscriptions _ =
    Sub.none
```
