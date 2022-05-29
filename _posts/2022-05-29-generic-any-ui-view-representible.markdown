---
layout: post
title:  "Generic AnyUIViewRepresentable"
date:   2022-05-29 08:40:13 -0400
categories: swift
---

There are situations where we want to wrap a UIView in a representible,
but we don't really want to have to create a new `UIViewRepresentible` for every view type.
In these cases, we can actually create a UIViewRepresentable with a generic constructor for such a situation.

```swift
struct AnyUIViewRepresentable: UIViewRepresentable {
    private let uiView: UIView
    
    init<View: UIView>(uiView: View) {
        self.uiView = uiView
    }
    
    func makeUIView(context: Context) -> UIView {
        uiView
    }
    
    func updateUIView(_ uiView: UIView, context: Context) {
        // do nothing
    }
}
```

Then, we could use it like so:

```swift
class SomeAdView: UIView {
    // some ad view implementation
}

struct SampleBody: View {
    var body: some View {
        Text("hello")
        AnyUIViewRepresentable(uiView: SomeAdView())
    }
}
```

The previous example does not expose the underlying UIView generically, which can make it easier to handle sometimes.

However, we could also expose the underlying ui view generically as well if we wanted:

```swift
struct AnyUIViewRepresentableExposed<View: UIView>: UIViewRepresentable {
    let uiView: View
    
    init(uiView: View) {
        self.uiView = uiView
    }
    
    func makeUIView(context: Context) -> UIView {
        uiView
    }
    
    func updateUIView(_ uiView: UIView, context: Context) {
        // do nothing
    }
}

struct SampleBodyExposed: View {
    var body: some View {
        Text("hello")
        AnyUIViewRepresentableExposed(uiView: SomeAdView())
    }
}
```

Depending on the situation, the non-exposed or exposed variant can provide utility.