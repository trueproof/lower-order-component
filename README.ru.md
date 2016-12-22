*На других языках: [English](README.md), [Русский](README.ru.md).*

1. Intro

 - После прочтения этой статьи вы можете потерять до 10% веса, а ваши отношения с домашним питомцем могут ухудшиться.
 - После прочтения этой статьи вы можете потерять до 10% премии, а ваши отношения с тимлидом могут ухудшиться.

 При написании этих 2 строк использовался паттерн copy-paste.

1. Code Reuse

 Чем меньше кода, тем меньше багов. Чтобы уменьшить количество кода применяют различные техники повторного использования кода. Важное наблюдение: "умные структуры данных и тупой код работает лучше, чем наоборот".

1. Parasite

 Определение: паразит определённое время использует хозяина в качестве источника питания и среды обитания, частично или полностью возлагая на него регуляцию своих взаимоотношений с окружающей средой.

1. Parasitic Inheritance

 http://www.crockford.com/javascript/inheritance.html

 Из всех паттернов самый запоминающийся для меня это Parasitic Inheritance, когда-то давно описанный моим профессиональным дедушкой. Легко запомнить название, которое вызывает мурашки, потерю аппетита и кошмары.

 Из этого паттерна можно выделить самого паразита
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
 который заражает хозяина и подменяет оригинальный, либо дополняет своим методом toString.

1. Object Augmentation

 В статье можно заметить, что Object Augmentation и Parasitic Inheritance практически совпадают: паттерн, названный Parasitic Inheritance является Object Augmentation, обёрнутым в конструктор. Т.о. Object Augmentation так же является заражением хозяина тем же паразитом.

1. Coupling

 Паразит может существовать только на хозяине, у которого определены методы `getValue` и `uber`. Если их нет, то он умирает вместе со своим заражённым хозяином при попытке задействовать свой функционал.

1. Mixin

 Приведённый выше паразит используется как миксин.

1. React

 React довольно гибок, но имеет единственное строгое правило: все React компоненты должны относиться к своим пропсам как чистые функции.

1. React Mixin

 React миксины накладываются на основной класс и дополняют его своими методами. Lifecycle-хуки вызываются в порядке вставки, остальные методы не могут быть определены больше 1 раза, поэтому приходится придумывать уникальные названия методов. Как миксины могут зависеть от основного компонента, так и основной компонент может зависеть от миксинов.

1. Class Component

 Самодостаточный строительный кирпичик интерфейса. Получает данные и колбеки из пропсов, может иметь собственный внутренний стейт.

1. Functional Component

 Не имеет собственного стейта. Объявляется как функция, принимающая пропс в качестве аргумента. В JSX выглядит так же, как обычный компонент.

1. Higher Order Component

 Компонент-хищник, который поглощает компонент-жертву и возвращает новый компонент, в котором может быть добавлен, убран, изменён фунционал жертвы. Может иметь свой стейт. Может иметь доступ к стейту, пропсам, методам жертвы. 2 основных типа: Props Proxy и Inheritance Inversion.

1. Lower Order Component

 Компонент-паразит, который не имеет собственного стейта, но имеет доступ к стейту, пропсам и методам хозяина через собственные пропсы. Манипуляции со стейтом хозяина производятся через `setState`. Методы паразита находятся в нём самом и не конфликтуют с методами хозяина.

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
                `After reading this article you may loss up to 10% of ${
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

