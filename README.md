# protobuf.tmbundle

Adds syntax highlighting and a few useful commands/snippets for Google's protocol buffer description language.

## Is that it?

For the most part. Though one neat feature is that - unlike most TextMate language descriptions - this one doesn't just look for keywords and highlight them. It actually builds up a mini-ADT that it highlights, and it *only* highlights things that fit its notion of a .proto file's structure. If you put this:

    optional int64 timestamp = 3;
    
outside of a message (or group, or extend...) declaration, it won't highlight. For the most part, this will be true: an rpc method declaration won't highlight except in a `Service` declaration.

Snippets are location-aware, so the `field` snippet (which generates a new message field) won't work except in a message. so using the `opt` snippet to generate an option will help you generate this in a message:

    message Foo {
      option message_set_wire_format = true;
    }

but in an RPC method, it will help you generate this:

    service FooService {
      method MyMethod() {
        option (method_option).timestamp = 123;
      }
    }

Notice how method options are structured slightly differently.

Also, TextMate has a shift-enter idiom for creating new functions/methods. At the top-level, this bundle will generate a new Message. Inside a message, this command will generate a new field. Inside an enum, it will generate a new enum value declaration (and automatically `UPCASE_AND_UNDERSCORE` the enum value name). Inside a Service declaration, it will generate a new rpc method.

These things are possible because this bundle actually understands the structure of the entire message, which is easy due to the intentionally-simple structure of .proto files.

## Installation

If only there were a straightforward way to install Bundles. Well, this always works:

    mkdir -p ~/"Library/Application Support/TextMate/Bundles/"
    cd ~/"Library/Application Support/TextMate/Bundles/"
    git clone git://github.com/michaeledgar/protobuf-tmbundle "Protocol Buffers.tmbundle"
    osascript -e 'tell app "TextMate" to reload bundles'

Pretty much all there is to it. All `.proto` files will be picked up by the bundle.

## Copyright

Copyright (c) 2010 Michael Edgar. See LICENSE for details.