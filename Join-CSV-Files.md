I have two CSV files. How to join them by one field?

With Gleam, this is easy. The implementation is in the gleamSort() function. It has some extra logic of only list out rows with different scores. But basically, we can load each CSV file as a dataset, and Join() them.

There is two other implementations using unix "sort" and "tr" tool. The "tr" is needed since the lines should be tab-separated values. I just write them to compare the performance.

It turns out the gleamSort() is about 10% faster. This could due to the additional data transformation needed to fit unix pipe format.

```
package main

import (
	"flag"
	"fmt"
	"os"
	"runtime/pprof"
	"time"

	"github.com/chrislusf/gleam"
	"github.com/chrislusf/gleam/source/csv"
)

var (
	a = flag.String("a", "a.csv", "first csv file with at least 5 fields, the first one being the key")
	b = flag.String("b", "b.csv", "second csv file with at least 5 fields, the first one being the key")
)

func main() {

	flag.Parse()

	timeIt("gleam:", *a, *b, gleamSort)
	timeIt("sort1:", *a, *b, directSort)
	timeIt("sort2:", *a, *b, unixSort)

}

func gleamSort(aFile, bFile string) {

	f := gleam.New()
	a := f.Input(csv.New(aFile).SetHasHeader(false)).Select(1, 2, 3, 5)
	b := f.Input(csv.New(bFile).SetHasHeader(false)).Select(1, 5)
	a.Join(b).Filter(`
      function(val1, val2, val3, score1, score2)
       return score1~=score2
     end
    `).Top(3).Fprintf(os.Stdout, "%s, %s, %s, %s, %s\n").Run()

}

func directSort(aFile, bFile string) {

	f := gleam.New()
	a := f.Input(csv.New(aFile).SetHasHeader(false)).Pipe("sort").Select(1, 2, 3, 5)
	b := f.Input(csv.New(bFile).SetHasHeader(false)).Pipe("sort").Select(1, 5)

	c := a.JoinPartitionedSorted(b, []int{1}, false, false)
	c.Filter(`
      function(val1, val2, val3, score1, score2)
        return score1~=score2
      end
    `).Top(3).Fprintf(os.Stdout, "%s, %s, %s, %s, %s\n").Run()

}

func unixSort(aFile, bFile string) {

	f := gleam.New()
	a := f.Strings([]string{aFile}).PipeAsArgs("sort $1").Pipe(`tr "," "\t"`).Select(1, 2, 3, 5)
	b := f.Strings([]string{bFile}).PipeAsArgs("sort $1").Pipe(`tr "," "\t"`).Select(1, 5)

	c := a.JoinPartitionedSorted(b, []int{1}, false, false)
	c.Filter(`
      function(val1, val2, val3, score1, score2)
        return score1~=score2
      end
    `).Top(3).Fprintf(os.Stdout, "%s, %s, %s, %s, %s\n").Run()

}

func timeIt(name string, aFile, bFile string, f func(aFile, bFile string)) {
	startTime := time.Now()
	fmt.Printf("%s start: %s\n", name, startTime)

	t, _ := os.Create(fmt.Sprintf("%s.pprof", name))
	pprof.StartCPUProfile(t)
	defer pprof.StopCPUProfile()

	f(aFile, bFile)

	fmt.Printf("%s time: %s\n\n", name, time.Now().Sub(startTime))
}

```