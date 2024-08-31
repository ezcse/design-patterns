# Adapter Design Pattern in C++

## Introduction

The Adapter Pattern is a structural design pattern that allows objects with incompatible interfaces to collaborate. It acts as a bridge between two incompatible interfaces by wrapping an existing class with a new interface, making it compatible with another class.

## Use Case

Imagine you have a class that expects a certain interface, but you have another class that provides the functionality you need but with a different interface. Instead of modifying the existing classes (which might not be possible), you create an Adapter class that makes the two classes work together.

## Structure

The Adapter Pattern typically involves the following components:

1. **Target Interface**: The interface that the client expects to work with.
2. **Adaptee**: The existing class that needs to be adapted to the Target Interface.
3. **Adapter**: A class that implements the Target Interface and translates the requests from the client to the Adaptee.

## Example

Let's consider an example where you have an existing media player that only supports playing MP3 files, but you want to play MP4 and VLC files without changing the existing media player code.

```cpp

#include <iostream>
#include <string>

// Target Interface
class MediaPlayer {
public:
    virtual void play(const std::string& audioType, const std::string& fileName) const = 0;
    virtual ~MediaPlayer() {}
};

// Adaptee Interface
class AdvancedMediaPlayer {
public:
    virtual void playMp4(const std::string& fileName) const = 0;
    virtual void playVlc(const std::string& fileName) const = 0;
    virtual ~AdvancedMediaPlayer() {}
};

// Concrete Adaptee: Mp4Player
class Mp4Player : public AdvancedMediaPlayer {
public:
    void playMp4(const std::string& fileName) const override {
        std::cout << "Playing mp4 file. Name: " << fileName << std::endl;
    }

    void playVlc(const std::string& fileName) const override {
        // Do nothing
    }
};

// Concrete Adaptee: VlcPlayer
class VlcPlayer : public AdvancedMediaPlayer {
public:
    void playMp4(const std::string& fileName) const override {
        // Do nothing
    }

    void playVlc(const std::string& fileName) const override {
        std::cout << "Playing vlc file. Name: " << fileName << std::endl;
    }
};

// Concrete Adaptee: Mp3Player
class Mp3Player : public MediaPlayer {
public:
    void play(const std::string& audioType, const std::string& fileName) const override {
        if (audioType == "mp3") {
            std::cout << "Playing mp3 file. Name: " << fileName << std::endl;
        }
    }
};

// Adapter Class
class MediaAdapter : public MediaPlayer {
private:
    AdvancedMediaPlayer* advancedPlayer;

public:
    MediaAdapter(const std::string& audioType) {
        if (audioType == "mp4") {
            advancedPlayer = new Mp4Player();
        } else if (audioType == "vlc") {
            advancedPlayer = new VlcPlayer();
        } else {
            advancedPlayer = nullptr;
        }
    }

    void play(const std::string& audioType, const std::string& fileName) const override {
        if (audioType == "mp4") {
            advancedPlayer->playMp4(fileName);
        } else if (audioType == "vlc") {
            advancedPlayer->playVlc(fileName);
        }
    }

    ~MediaAdapter() {
        delete advancedPlayer;
    }
};

// Client: AudioPlayer
class AudioPlayer : public MediaPlayer {
private:
    MediaAdapter* mediaAdapter;
    Mp3Player* mp3Player;

public:
    AudioPlayer() : mediaAdapter(nullptr), mp3Player(new Mp3Player()) {}

    void play(const std::string& audioType, const std::string& fileName) const override {
        if (audioType == "mp3") {
            mp3Player->play(audioType, fileName);
        } else if (audioType == "mp4" || audioType == "vlc") {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter->play(audioType, fileName);
            delete mediaAdapter;
        } else {
            std::cout << "Invalid media. " << audioType << " format not supported." << std::endl;
        }
    }

    ~AudioPlayer() {
        delete mp3Player;
    }
};

// Main function to demonstrate the Adapter Pattern
int main() {
    AudioPlayer player;
    player.play("mp3", "song.mp3");
    player.play("mp4", "video.mp4");
    player.play("vlc", "movie.vlc");
    player.play("avi", "file.avi");

    return 0;
}
```

## Benefits of Adapter Pattern

1. **Flexibility**: Allows you to integrate classes that couldn't otherwise work together.
2. **Reusability**: You can reuse existing classes with different interfaces without altering their code.
3. **Single Responsibility**: The Adapter class focuses on resolving interface mismatches, keeping other classes free of this concern.

## How to Identify the Adapter Pattern

You can identify the Adapter Pattern in code when:

- You see a class (`Adapter`) that implements an interface expected by a client (`Target Interface`), but internally uses another class (`Adaptee`) to perform the actual work.
- The `Adapter` class is used to "adapt" an existing class with an incompatible interface to match the required interface.
- There are often multiple incompatible classes (e.g., `Mp4Player`, `VlcPlayer`), and the adapter can wrap these classes to make them compatible with a single client.

### Signs of Adapter Pattern in Code

- The presence of a class that implements a known interface but internally delegates calls to a different class.
- Code where existing classes or APIs are being adapted to a new interface without modifying the original classes.
- The use of composition over inheritance, where the adapter holds a reference to the adaptee.

## Conclusion

The Adapter Pattern is a powerful tool in your design pattern toolkit, allowing you to integrate and reuse classes with incompatible interfaces. By identifying the need for an Adapter, you can avoid modifying existing classes and instead wrap them with a class that translates the interface, keeping your code flexible and maintainable.
