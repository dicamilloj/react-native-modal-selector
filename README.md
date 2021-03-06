# react-native-modal-selector [![npm version](https://badge.fury.io/js/react-native-modal-selector.svg)](https://badge.fury.io/js/react-native-modal-selector)

A cross-platform (iOS / Android), selector/picker component for React Native that is highly customizable and supports sections.

This project is the official continuation of the abandoned `react-native-modal-picker` repo. Contributors are welcome to [request a promotion to collaborator status](https://github.com/peacechen/react-native-modal-selector/issues/1).

## Demo

<img src="https://github.com/peacechen/react-native-modal-selector/blob/master/docs/demo.gif" />

## Install

```sh
npm i react-native-modal-selector --save
```

## Usage

You can either use this component in its default mode, as a wrapper around your existing component or provide a custom component (where you need to control opening of the modal yourself). In default mode a customizable button is rendered.

See `SampleApp` for an example how to use this component.

```jsx

import ModalSelector from 'react-native-modal-selector'

class SampleApp extends Component {

    constructor(props) {
        super(props);

        this.state = {
            textInputValue: ''
        }
    }

    render() {
        let index = 0;
        const data = [
            { key: index++, section: true, label: 'Fruits' },
            { key: index++, label: 'Red Apples' },
            { key: index++, label: 'Cherries' },
            { key: index++, label: 'Cranberries', accessibilityLabel: 'Tap here for cranberries' },
            // etc...
            // Can also add additional custom keys which are passed to the onChange callback
            { key: index++, label: 'Vegetable', customKey: 'Not a fruit' }
        ];

        return (
            <View style={{flex:1, justifyContent:'space-around', padding:50}}>

                // Default mode
                <ModalSelector
                    data={data}
                    initValue="Select something yummy!"
                    onChange={(option)=>{ alert(`${option.label} (${option.key}) nom nom nom`) }} />

                // Wrapper
                <ModalSelector
                    data={data}
                    initValue="Select something yummy!"
                    supportedOrientations={['landscape']}
                    accessible={true}
                    scrollViewAccessibilityLabel={'Scrollable options'}
                    cancelButtonAccessibilityLabel={'Cancel Button'}
                    onChange={(option)=>{ this.setState({textInputValue:option.label})}}>

                    <TextInput
                        style={{borderWidth:1, borderColor:'#ccc', padding:10, height:30}}
                        editable={false}
                        placeholder="Select something yummy!"
                        value={this.state.textInputValue} />

                </ModalSelector>

                // Custom component
                <ModalSelector
                    data={data}
                    ref={selector => { this.selector = selector; }}
                    customSelector={<Switch onValueChange={() => this.selector.open()} />}
                />
            </View>
        );
    }
}
```

## Data Format

The selector accept a specific format of data:
```javascript
[{ key: 5, label: 'Red Apples' }]
```

If your data has a specific format, you can define extractors of data, example:
```javascript
this.setState({data: [{ id: 5, name: 'Red Apples' }]});

return (
  <ModalSelector
    data={this.state.data}
    keyExtractor= {item => item.id}
    labelExtractor= {item => item.name}
  />
);
```


## API
### Props
Prop                | Type     | Optional | Default      | Description
------------------- | -------- | -------- | ------------ | -----------
`data`              | array    | No       | []           | array of objects with a unique key and label to select in the modal.
`onChange`          | function | Yes      | () => {}     | callback function, when the users has selected an option
`onModalOpen`       | function | Yes      | () => {}     | callback function, when modal is opening
`onModalClose`      | function | Yes      | () => {}     | callback function, when modal is closing
`keyExtractor`      | function | Yes      | (data) => data.key   | extract the key from the data item
`labelExtractor`    | function | Yes      | (data) => data.label | extract the label from the data item
`visible`           | bool     | Yes      | false        | control open/close state of modal
`closeOnChange`´    | bool     | Yes      | true         | control if modal closes on select
`initValue`         | string   | Yes      | `Select me!` | text that is initially shown on the button
`cancelText`        | string   | Yes      | `cancel`     | text of the cancel button
`animationType`     | string   | Yes      | `slide`      | type of animation to be used to show the modal. Must be one of `none`, `slide` or `fade`.
`disabled`          | bool     | Yes      | false        | `true` disables opening of the modal
`childrenContainerStyle`| object   | Yes      | {}           | style definitions for the children container view
`touchableStyle`    | object   | Yes      | {}           | style definitions for the touchable element
`touchableActiveOpacity`    | number   | Yes      | 0.2           | opacity for the touchable element on touch
`supportedOrientations`    | ['portrait', 'landscape'] | Yes      | both      | orientations the modal supports
`keyboardShouldPersistTaps`| `string` / `bool`         | Yes      | `always`  | passed to underlying ScrollView
`style`             | object   | Yes      |              | style definitions for the root element
`selectStyle`       | object   | Yes      | {}           | style definitions for the select element (available in default mode only!). NOTE: Due to breaking changes in React Native, RN < 0.39.0 should pass `flex:1` explicitly to `selectStyle` as a prop.
`selectTextStyle`   | object   | Yes      | {}           | style definitions for the select element (available in default mode only!)
`overlayStyle`      | object   | Yes      | { flex: 1, padding: '5%', justifyContent: 'center', backgroundColor: 'rgba(0,0,0,0.7)' } | style definitions for the overlay background element. RN <= 0.41 should override this with pixel value for padding.
`sectionStyle`      | object   | Yes      | {}           | style definitions for the section element
`sectionTextStyle`  | object   | Yes      | {}           | style definitions for the select text element
`selectedItemTextStyle` | object | Yes    | {}           | style definitions for the currently selected text element
`optionStyle`       | object   | Yes      | {}           | style definitions for the option element
`optionTextStyle`   | object   | Yes      | {}           | style definitions for the option text element
`optionContainerStyle`| object | Yes      | {}           | style definitions for the option container element
`cancelStyle`       | object   | Yes      | {}           | style definitions for the cancel element
`cancelTextStyle`   | object   | Yes      | {}           | style definitions for the cancel text element
`cancelContainerStyle`| object | Yes      | {}           | style definitions for the cancel container
`backdropPressToClose`| bool   | Yes  | false        | `true` makes the modal close when the overlay is pressed
`passThruProps`| object   | Yes  | {}        | props to pass through to the container View and each option TouchableOpacity (e.g. testID for testing)
`accessible`| bool   | Yes  | false        | `true` enables accessibility for modal and options. Note: data items should have an `accessibilityLabel` property if this is enabled
`scrollViewAccessibilityLabel` | string   | Yes      | undefined | Accessibility label for the modal ScrollView
`cancelButtonAccessibilityLabel` | string   | Yes      | undefined | Accessibility label for the cancel button
`modalOpenerHitSlop` | object | Yes | {} | How far touch can stray away from touchable that opens modal ([RN docs](https://facebook.github.io/react-native/docs/touchablewithoutfeedback.html#hitslop))
`customSelector`     | node   | Yes | undefined          | Render a custom node instead of the built-in select box.
