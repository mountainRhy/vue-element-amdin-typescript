## Vue模板语言中使用TypeScript

### 一、项目相关

本项目是基于github中的`PanJiaChen`的`vue-element-admin`项目之上进行修改开发，感谢项目贡献者，相关说明文档及API说明已经在本文档中进行了说明。

```bash

git clone https://gitlab.coohua.com/coohua-fe/admin-common.git

cd admin-common

安装依赖
yarn (推荐使用yarn来安装依赖，很少会出问题)

启动项目
yarn start OR npm run serve

build正式环境
yarn build:prod

build stage环境
yarn build:stage

打包之后会打包至dist文件夹。

```

### 二、代码一览

```js
@Component({
  name: 'Temp'
  props: {
    propMessage: String
  }
})

export default class extends Vue {

  // data
  private data = ''
  private watcher = 1
  
  // computed
  get computer() {
    return PermissionModule.routes
  }
  
  // watch
  @Watch('watcher')
  private onWatcherChange() {
    console.log('变化了')
  }

  /* 
    生命周期
    beforeUpdate
    updated
    activated
    deactivated
    beforeDestroy
    destroyed
    errorCaptured
  */
  mounted() {
    console.log('生命周期触发')
  }
  
  // methods
  private methodf(index: number, obj: object) {
    console.log(index, obj)
  }
}
```

### 三、API说明

1. 上述代码中表明了对应位置对应VUE中的API
2. TS定义
    ```js
    // 布尔值
    let isDone: boolean = false; // 相当于 js 的 let isDone = false;
    // 变量定义之后不可以随便变更它的类型
    isDone = true // 不报错
    isDone = "我要变为字符串" // 报错
    // 数字
    let decLiteral: number = 6; // 相当于 js 的   let decLiteral = 6;
    // 字符串
    let name: string = "bob";  // 相当于 js 的 let name = "bob";
    // 数组
    // 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：
    let list: number[] = [1, 2, 3]; // 相当于 js 的let list = [1, 2, 3];
    // 第二种方式是使用数组泛型，Array<元素类型>：
    let list: Array<number> = [1, 2, 3]; // 相当于 js 的let list = [1, 2, 3];
    // 在 TypeScript 中，我们使用接口（Interfaces）来定义 对象 的类型。
    interface Person {
        name: string;
        age: number;
    }
    let tom: Person = {
        name: 'Tom',
        age: 25
    };
    // 以上 对象 的代码相当于 
    let tom = {
        name: 'Tom',
        age: 25
    };
    // Any 可以随便变更类型 (当这个值可能来自于动态的内容，比如来自用户输入或第三方代码库)
    let notSure: any = 4;
    notSure = "我可以随便变更类型" // 不报错
    notSure = false;  // 不报错
    // Void 当一个函数没有返回值时，你通常会见到其返回值类型是 void
    function warnUser(): void {
        console.log("This is my warning message");
    }
    // 方法的参数也要定义类型，不知道就定义为 any
    function fetch(url: string, id : number, params: any): void {
        console.log("fetch");
    }
    ```
    
3. 装饰器Emit
    ```js
    @Emit()
    addToCount(n: number) {
        this.count += n
    }
    // 相当于
    addToCount(n) {
      this.count += n
      this.$emit('add-to-count', n)
    }
    ```
4. 装饰器Inject
    ```js
      @Inject() readonly foo!: string
      @Inject('bar') readonly bar!: string
      @Inject({ from: 'optional', default: 'default' }) readonly optional!: string
      @Inject(symbol) readonly baz!: string
      // 相当于
      inject: {
        foo: 'foo',
        bar: 'bar',
        optional: { from: 'optional', default: 'default' },
        [symbol]: symbol
      },
    ```
5. 装饰器Model
    ```js
        @Model('change', { type: Boolean }) readonly checked!: boolean
        // 相当于
        model: {
            prop: 'checked',
            event: 'change'
        },
        props: {
            checked: {
                type: Boolean
            }
        }
    ```
6. 装饰器Prop
    ```js
        @Prop(Number) readonly propA: number | undefined
        @Prop({ default: 'default value' }) readonly propB!: string
        @Prop([String, Boolean]) readonly propC: string | boolean | undefined
        // 相当于
        props: {
            propA: {
                type: Number
            },
            propB: {
                default: 'default value'
            },
            propC: {
                type: [String, Boolean]
            }
        }
    ```
7. 装饰器Provide
    ```js
        @Inject() readonly foo!: string
        @Inject('bar') readonly bar!: string
        @Inject({ from: 'optional', default: 'default' }) readonly optional!: string
        @Inject(symbol) readonly baz!: string
        
        @Provide() foo = 'foo'
        @Provide('bar') baz = 'bar'
        // 相当于
        inject: {
            foo: 'foo',
            bar: 'bar',
            optional: { from: 'optional', default: 'default' },
            [symbol]: symbol
        },
        data() {
            return {
                foo: 'foo',
                baz: 'bar'
            }
        },
        provide() {
            return {
                foo: this.foo,
                bar: this.baz
            }
        }
    ```
8. 装饰器Watch
    ```js
    @Watch('child')
    onChildChanged(val: string, oldVal: string) {}

    @Watch('person', { immediate: true, deep: true })
    onPersonChanged1(val: Person, oldVal: Person) {}

    @Watch('person')
    onPersonChanged2(val: Person, oldVal: Person) {}
    // 相当于
    watch: {
        child: [
            {
                handler: 'onChildChanged',
                immediate: false,
                deep: false
            }
        ],
        person: [
            {
                handler: 'onPersonChanged1',
                immediate: true,
                deep: true
            },
            {
                handler: 'onPersonChanged2',
                immediate: false,
                deep: false
            }
        ]
    },
    methods: {
        onChildChanged(val, oldVal) {},
        onPersonChanged1(val, oldVal) {},
        onPersonChanged2(val, oldVal) {}
    }
    ```
9. 装饰器Component
    ```js
        @Component
    ```
10. 详细API说明
    <https://github.com/kaorun343/vue-property-decorator>
    

