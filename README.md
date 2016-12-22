*In other languages: [English](README.md), [Русский](README.ru.md).*

1. Intro

 - After reading this article you may lose up to 10% of weight, and your relationship with your pet may get worse.
 - After reading this article you may lose up to 10% of bonus, and your relationship with your teamlead may get worse.

 For writing these 2 lines copy-paste pattern was used.

1. Code Reuse

 The less code, the less bugs. To lessen amount of code various techniques of code reuse are used. Important observation: "smart data structures and dumb code works a lot better than the other way around".

1. Parasite

 Definition: parasite uses its host for some time as food source and habitat, partially or completely delegating regulation of own relationship with environment to it.

1. Parasitic Inheritance

 http://www.crockford.com/javascript/inheritance.html

 Of all patterns the most memorable to me is Parasitic Inheritance, once upon a time described by my professional grandfather. It is easy to keep in mind a name, which cause creeps, anorexia and nightmares.

 It is possible to extract the parasite from that example
    ```
    {
        toString() {
            if (this.getValue()) {
                return this.uber('toString')
            }
            return '-0-'
        }
    }
    ```
 which infects the host and replaces original or adds its own method `toString`.

1. Object Augmentation

 It can be seen in article, that Object Augmentation and Parasitic Inheritance mostly coincide: pattern named Parasitic Inheritance is Object Augmentation, wrapped by a constructor. I.e. Object Augmentation is also host infection by the same parasite.

1. Coupling

 Parasite may exist only on a host, which has `getValue` and `uber` methods defined. If they are missing, then it dies along with its infected host in an attempt to activate its functionality.

1. Mixin

 The parasite shown above is used as mixin.

1. React

 React is pretty flexible but it has a single strict rule: all React components must act like pure functions with respect to their props.

1. React Mixin

 React mixins are layed upon main class and augment it with its own methods. Lifecycle-hooks are invoked in insertion order, other methods cannot be defined more than once, so it is required to think up unique method names. As mixins may depend on main component, so may main component depend on mixins.

1. Class Component

 Self-sufficient building block of interface. Gets data and callbacks from props, may have its own inner state.

1. Functional Component

 Doesn't have own state. Is defined as function, receiving props as argument. In JSX looks just like usual component.

1. Higher Order Component

 Component-predator, which consumes a component-victim and returns a new component. In that new component victim's functionality may be added, removed, altered. May have own state. May have access to victim's state, props, methods. Have 2 main types: Props Proxy and Inheritance Inversion.

1. Lower Order Component

 Component-parasite, which has no own state, but has access to state, props and methods of the host through own props. Host's state manipulation is performed through `setState`. Parasite's methods are placed in itself and there's no conflict with host's methods.

1. Code

    ```
    const Host = React.createClass({
        getInitialState() {
            return {
                loss: 'weight',
                partner: 'pet'
            }
        },

        render() {
            return (
                <div>
                    <Parasite
                        self={this}
                    />
                    <Button
                        label="weight"
                        onClick={this.setWeightAndCat}
                    />
                    <Button
                        label="bonus"
                        onClick={this.setBonusAndTeamlead}
                    />
                </div>
            )
        },

        setWeightAndCat() {
            this.setState({
                loss: 'weight',
                partner: 'pet'
            })
        },

        setBonusAndTeamlead() {
            this.setState({
                loss: 'bonus',
                partner: 'teamlead'
            })
        },
    })

    const Parasite = React.createClass({
        render() {
            const {self} = this.props
            const {loss, partner} = self.state

            return (
                <div>
                    <Phrase
                        loss={loss}
                        partner={partner}
                    />
                    <Button
                        label="upper"
                        onClick={this.upper}
                    />
                </div>
            )
        },

        upper() {
            const {self} = this.props
            const {loss, partner} = self.state

            self.setState({
                loss: loss.toUpperCase(),
                partner: partner.toUpperCase()
            })
        }
    })

    function Phrase(props) {
        const {loss, partner} = props

        return (
            <div>
            {
                `After reading this article you may lose up to 10% of ${
                    loss
                }, and your relationship with your ${
                    partner
                } may get worse.`
            }
            </div>
        )
    }

    function Button(props) {
        const {label, onClick} = props

        return (
            <button
                onClick={onClick}
            >
                {label}
            </button>
        )
    }
    ```
