# React Native Animated Glow v4

<div align="center">

[![NPM Version](https://img.shields.io/npm/v/react-native-animated-glow.svg)](https://www.npmjs.com/package/react-native-animated-glow)
[![NPM License](https://img.shields.io/npm/l/react-native-animated-glow.svg)](https://github.com/realimposter/react-native-animated-glow/blob/main/LICENSE)
[![NPM Downloads](https://img.shields.io/npm/dt/react-native-animated-glow.svg)](https://www.npmjs.com/package/react-native-animated-glow)

</div>

A performant animated glow effect for React Native, powered by **Skia** and **Reanimated 4**.

`react-native-animated-glow` v4 targets the Expo 55 consumer baseline:

- React Native `0.83.x`
- React `19.2`
- `@shopify/react-native-skia` `2.4.x`
- `react-native-reanimated` `4.2.x`
- `react-native-worklets` `0.7.x`

v4 is **New Architecture only**. iOS and Android are the primary supported platforms. Web remains **experimental**.

![React Native Animated Glow Demo](https://raw.githubusercontent.com/realimposter/react-native-animated-glow/main/assets/react-native-glow-demo.gif)

## Installation

### Expo 55

Install the package in your Expo app:

```bash
npm install react-native-animated-glow
npx expo install @shopify/react-native-skia react-native-reanimated react-native-worklets
```

Expo configures the Worklets Babel plugin for you.

### Bare React Native / Community CLI

Install the package and its peer dependencies:

```bash
npm install react-native-animated-glow @shopify/react-native-skia react-native-reanimated react-native-worklets
```

Add the Worklets Babel plugin and keep it last:

```js
module.exports = {
  presets: ['module:@react-native/babel-preset'],
  plugins: [
    'react-native-worklets/plugin',
  ],
};
```

Then follow the native installation steps from:

- [React Native Skia docs](https://shopify.github.io/react-native-skia/docs/getting-started/installation)
- [Reanimated docs](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started/)
- [React Native Worklets docs](https://docs.swmansion.com/react-native-worklets/docs/)

## Usage

The recommended API is a `PresetConfig` with a `states` array.

```tsx
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import AnimatedGlow, { type PresetConfig } from 'react-native-animated-glow';

const preset: PresetConfig = {
  metadata: {
    name: 'CTA',
    textColor: '#FFFFFF',
    category: 'Custom',
    tags: ['interactive'],
  },
  states: [
    {
      name: 'default',
      preset: {
        cornerRadius: 28,
        outlineWidth: 2,
        borderColor: ['#FFF275', '#5EF38C', '#60A5FA', '#F472B6'],
        backgroundColor: '#111111',
        glowLayers: [
          {
            glowPlacement: 'behind',
            colors: ['#FFF275', '#5EF38C', '#60A5FA', '#F472B6'],
            glowSize: 24,
            opacity: 0.28,
            speedMultiplier: 1,
            coverage: 1,
          },
        ],
      },
    },
    {
      name: 'hover',
      transition: 240,
      preset: {
        glowLayers: [{ glowSize: 30, opacity: 0.34 }],
      },
    },
    {
      name: 'press',
      transition: 120,
      preset: {
        glowLayers: [{ glowSize: 34, opacity: 0.4 }],
      },
    },
  ],
};

export function Example() {
  return (
    <AnimatedGlow preset={preset}>
      <View style={styles.button}>
        <Text style={styles.text}>Press me</Text>
      </View>
    </AnimatedGlow>
  );
}

const styles = StyleSheet.create({
  button: {
    paddingHorizontal: 24,
    paddingVertical: 16,
    backgroundColor: '#111111',
  },
  text: {
    color: '#FFFFFF',
    fontWeight: '600',
  },
});
```

## Controlled States

`AnimatedGlow` is controlled via `activeState`. Pair it with a `Pressable` if you want to drive `default`, `hover`, and `press` explicitly.

```tsx
import React, { useRef, useState } from 'react';
import { Pressable, Text } from 'react-native';
import AnimatedGlow, { glowPresets, type GlowEvent } from 'react-native-animated-glow';

export function InteractiveExample() {
  const [state, setState] = useState<GlowEvent>('default');
  const isHovered = useRef(false);

  return (
    <AnimatedGlow preset={glowPresets.defaultRainbow} activeState={state}>
      <Pressable
        onPressIn={() => setState('press')}
        onPressOut={() => setState(isHovered.current ? 'hover' : 'default')}
        onHoverIn={() => {
          isHovered.current = true;
          if (state !== 'press') {
            setState('hover');
          }
        }}
        onHoverOut={() => {
          isHovered.current = false;
          if (state !== 'press') {
            setState('default');
          }
        }}
      >
        <Text>Tap or hover me</Text>
      </Pressable>
    </AnimatedGlow>
  );
}
```

## Web

Web support is experimental and not part of the v4 release gate. If you need web, make sure your app also follows the current Skia web installation guidance for CanvasKit.

## Migration from v3

v4 is an intentional major break.

- The package now targets the Expo 55 / React Native `0.83` stack.
- The package requires `react-native-worklets` alongside Reanimated 4.
- The package is New Architecture only.
- `react-native-gesture-handler` is no longer part of the package install contract.

If you need the older dependency line, stay on v3.

## License

MIT
