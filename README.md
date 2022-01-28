# swiftui-shimmer-text-anim

```swift

//
//  ContentView.swift
//  SwiftUI Text Shimmer Effect
//
//  Created by paige on 2022/01/28.
//

import SwiftUI

struct ContentView: View {
    var body: some View {
        Home()
            .colorScheme(.dark)
    }
}


struct Home: View {
    
    // Toggle For MultiColors...
    @State var multiColor: Bool = false
    
    var body: some View {
        
        VStack(spacing: 25) {
        
            TextShimmer(text: "Paige", multiColor: $multiColor)
        
            Toggle(isOn: $multiColor) {
                Text("Enable Multi Colors")
                    .font(.title)
                    .fontWeight(.bold)
            }
            .padding()
        
        }
        
    }
    
}

// TextShimmer ...
struct TextShimmer: View {
    
    var text: String
    @State var animate: Bool = false
    @Binding var multiColor: Bool
    
    var body: some View {
        
        ZStack {
            Text(text)
                .font(.system(size: 75, weight: .bold))
                .foregroundColor(.white.opacity(0.25))
            
            // MultiColor Text...
            HStack(spacing: 0) {
                ForEach(0..<text.count, id: \.self) { index in
                    // 각각의 텍스트를 하나씩 출력
                    Text(String(text[text.index(text.startIndex, offsetBy: index)]))
                        .font(.system(size: 75, weight: .bold))
                        .foregroundColor(multiColor ? randomColor() : .none)
                }
            } //: HSTACK
            // Masking for Shimmer Effect...
            .mask(
                Rectangle()
                // For some more nice effect, add gradient
                    .fill(
                        // You can use any Color Here...
                        LinearGradient(gradient: .init(colors: [Color.white.opacity(0.5), Color.white, Color.white.opacity(0.5)]), startPoint: .top, endPoint: .bottom)
                    )
                    .rotationEffect(.init(degrees: 70))
                    .padding(20)
                // Moving View Continuously
                // So it will create Shimmer Effect...
                    .offset(x: -250)
                    .offset(x: animate ? 500 : 0)
            )
            .onAppear {
                withAnimation(.linear(duration: 2).repeatForever(autoreverses: false)) {
                    animate.toggle()
                }
            }
        }
        
    }
    
    // Random Color
    // It's your wish you can change anything here...
    // or you can also use array of colors to pcik random one...
    func randomColor() -> Color {
        let color: UIColor = UIColor(red: CGFloat.random(in: 0...1), green: CGFloat.random(in: 0...1), blue: CGFloat.random(in: 0...1), alpha: 1)
        return Color(color)
    }
    
}


```
