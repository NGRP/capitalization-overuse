---
id: withForm-1
title: Forms
sidebar_label: Forms
---
## [WIP] withForm 
An HOC to use in your forms to simplify the input onChange keeping the state updated, you can although set up a validation predicate 
### HOC Sample
```javascript
import React, { Component } from 'react';

const formEnhancer = (validationPredicate = true) => WrappedComponent =>
    class extends Component {
        constructor(props) {
            super(props);
            this.state = {
                fields: props.initialFields || {},
                formValid: true
            };
        }
        initFields = value => {
            this.setState({ fields: value });
        };
        checkValidity = () => {
            const formValid = validationPredicate(this.state.fields);
            this.setState({ formValid });

            return formValid;
        };

        setField = field => text => {
            this.setState(state => ({
                fields: {
                    ...state.fields,
                    [field]: text
                }
            }));
        };

        render() {
            return (
                <WrappedComponent
                    {...this.props}
                    fields={this.state.fields}
                    setField={this.setField}
                    checkValidity={this.checkValidity}
                    formValid={this.state.formValid}
                    initFields={this.initFields}
                />
            );
        }
    };

export const withForm = formEnhancer;
```
### Usage sample
```javascript 1.8
import React, { Component } from 'react';
import { View, Button, TextInput } from 'react-native';
import { withForm } from '../enhancers/withForm';

const validationPredicate = fields => fields.email && fields.password;

class PureSampleForm extends Component {
    submit = () => {
        const { checkValidity } = this.props;
        if (checkValidity()) {
            // form is valid
        }
    };

    render() {
        const { fields, setField, formValid } = this.props;
        // formsValid is updated when checkValidity() is called
        return (
            <View>               
                <TextInput    
                    placeholder="email"
                    onChangeText={setField('email')}
                />
                <TextInput                        
                    onChangeText={setField('password')}
                    placeholder="password"
                    secureTextEntry={true}                        
                /> 
                <Button                    
                    text="Submit Action"
                    onPress={this.submit}
                />
            </View>
        );
    }    
}

export const SampleForm = withForm(validationPredicate)(PureSignInForm);

```
