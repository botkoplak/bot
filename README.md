package main
 
import (
    "log"
    "os"
 
    "github.com/fujiwara/ridge"
    "github.com/kawaken/go-linebot-wrapper"
    "github.com/line/line-bot-sdk-go/linebot"
)
 
func main() {
    logger := log.New(os.Stderr, "", log.Lshortfile)
 
    h, err := wrapper.New(os.Getenv("CHANNEL_SECRET"), os.Getenv("CHANNEL_TOKEN"))
    if err != nil {
        logger.Print(err)
        return
    }
 
    h.OnTextMessageRecieved = func(e *linebot.Event, m *linebot.TextMessage) []linebot.Message {
        return []linebot.Message{
            &linebot.TextMessage{Text: m.Text},
        }
    }
 
    h.OnBeaconEnter = func(e *linebot.Event, hwid string) []linebot.Message {
        return []linebot.Message{
            &linebot.TextMessage{Text: "Enter"},
        }
    }
 
    h.OnBeaconLeave = func(e *linebot.Event, hwid string) []linebot.Message {
        return []linebot.Message{
            &linebot.TextMessage{Text: "Leave"},
        }
    }
 
    ridge.Run(":8080", "/callback", h)
}
