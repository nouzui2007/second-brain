# Pluginsでdispatchを使う

```jsx
export default function ({ store, redirect }) {
    // ユーザ認証されてない時
    if (!store.getters["auth/authenticated"]) {
        console.log("no signed in")
        return redirect('/signin')
    } else {
        const callback = (user) => {
            if (user) {
                store.dispatch("auth/setUser", { user: user })
            }
        }
        store.dispatch("auth/onAuthStateChanged", { callback: callback })
    }
}
```