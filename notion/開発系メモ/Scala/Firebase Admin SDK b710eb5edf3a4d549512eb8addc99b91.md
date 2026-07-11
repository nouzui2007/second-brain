# Firebase Admin SDK

## ApiFuture

コールバックで処理する

```scala
def methodname(): Future[String] = {
	val p = Promise[String]()
	ApiFutures.addCallback(
	  res,
	  new ApiFutureCallback[DocumentSnapshot] {
	    def onFailure(t: Throwable) = p failure t
	    def onSuccess(result: DocumentSnapshot) =
	      p success result.get("token").toString()
	  }
	)
	p.future
}
```