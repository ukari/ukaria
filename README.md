# aria
data structures and some trival stuffs in common lisp for my lady UK Aria H. Kanzaki

## aria.asynchronous.scheduler
a scheduler, works in a separate thread, takes tasks into queue and consume task one by one

### provides
- `(defclass scheduler ())`
- `(defun gen-scheduler (&key (onclose nil) (name "Anonymous scheduler thread")))`
- `(defmethod add ((self scheduler) (task function)))`
- `(defmethod end ((self scheduler)))`

## aria.asynchronous.timer
a timer based on scheduler, basically used for provide a `settimeout`

### provides
- `(defclass timer ())`
- `(defun gen-timer (&key (scheduler (gen-scheduler))))`
- `(defmethod settimeout ((self timer) (callback function) &optional (milliseconds 0)))`
- `(defmethod end ((self timer))`
- `(defmethod cleartimeout ((clear function)))`

## aria.control.rx
frp for cl inspired by [reactivex](http://reactivex.io/)

[staltz's introduce of frp](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

### provides
- `(defclass observable ())`
- `(defclass observer ())`
- `(defclass subject (observer))`
- `(defmethod onnext ((self observer)))`
- `(defmethod onfail ((self observer)))`
- `(defmethod onover ((self observer)))`
- `(defmethod subscribe ((self observable) (ob observer)))`
- `(defmethod subscribe ((self observable) (onnext function)))`
- `(defmethod subscribe ((self subject) (ob observer)))`
- `(defmethod subscribe ((self subject) (onnext function)))`
- `(defmethod operator ((self observable) (pass function)))`

### provide operators
- `(defmethod mapper ((self observable) (function function)))`
- `(defmethod mapto ((self observable) value))`
- `(defmethod each ((self observable) (consumer function)))`
- `(defmethod filter ((self observable) (predicate function)))`
- `(defmethod debounce ((self observable) (timer function) (clear function)))`
- `(defmethod throttle ((self observable) (onservablefn function)))`

## aria.structure.queue
Just a normal queue

### provides
- `(defstruct queue)`
- `(defun make-queue())`
- `(defmethod en ((self queue) e))`
- `(defmethod de ((self queue)))`
- `(defmethod queue-empty-p ((self queue))`

## aria.structure.miso-queue
A multi-in-single-out queue

### about
This miso queue is designed for Multi-In-Single-Out situation, the operation `de`(means dequeue) is not thread safe due to it should only be excuted in a single thread

### provides
- `(defstruct queue)`
- `(defun make-queue())`
- `(defmethod en ((self queue) e))`
- `(defmethod de ((self queue)))`
- `(defmethod queue-empty-p ((self queue))`

## aria.structure.mimo-queue
A multi-in-multi-out queue

### about
This miso queue is designed for Multi-In-Multi-Out situation, methods `en`, `de`, `queue-empty-p` are all thread safe.

### provides
- `(defstruct queue)`
- `(defun make-queue())`
- `(defmethod en ((self queue) e))`
- `(defmethod de ((self queue)))`
- `(defmethod queue-empty-p ((self queue))`

## aria.structure.pair-heap
A pairing heap, [wiki](https://en.wikipedia.org/wiki/Pairing_heap)

### about
In default options, `(make-heap)` will generate a min pairing heap with number type elements.

With the help of `(make-heap)`'s option `:compare` and `:accessor`, the heap could expand to sort by a user defined way.

### provides
- `(defstruct pair-heap)`
- `(defmethod make-heap (&key (element nil) (compare (lambda (x y) (< x y)) (accessor (lambda (x) x)))`
- `(defmethod en ((self pair-heap) e))`
- `(defmethod de ((self pair-heap)))`
- `(defmethod heap-empty-p ((self pair-heap)))`
- `(defmethod find-top ((self pair-heap)))`
