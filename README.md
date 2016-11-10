# CILK Exercise - Quicksort

## Getting started with exercise

1. Open a terminal and clone your repository on to your local machine using `git clone`.

2. Navigate to the assignment directory and create a new folder `build` and navigate into it i.e.
    ```bash
    cd assignment-skanur
    mkdir build
    cd build
    ```

3. Two compilers are provided - LLVM version of Cilk and Intel Cilk compiler. You can switch to LLVM version of Cilk Plus using 
    ```bash
    use cilkplus
    ```

    Intel version of Cilk tools can be accessed using
    ```
    use parallelstudio
    ```
4. To compile the code and run, navigate to `build` directory and 
    ```bash
    cmake ..
    make
    ./qsort 1000000
    ```

5. You can measure system usage during the code execution using the script `measure.sh`. Execute this script by using appropriate compiler environment and navigating back to your project folder and running
    ```bash
    ./measure.sh
    ```
    This will print out CPU utilization, frequency and temperature of all the cores every second as well as the average of the values once the execution is finished.

6. Work on the code using your favorite text editor or you can also use `kdevelop`. Make local commits on points you think are important (for e.g. various parallelisation strategies). Push it to remote when you think necessary. 

7. Once completed with the assignment, create a tag **final** for your master branch

8. Push the repository to remote using `git push --follow-tags -u origin master` before the exercise deadline of **24th November 2016 23:55:00**.

9. **WARNING**: This repo will be deleted after the exercise assessment without any prior notice. If you want a copy of it, fork it **privately**.

## Exercise Description

Quicksort is a very efficient sorting algorithm. For more information take a look at Wikipedia for example. In Wikipedia, the following pseudo-code is stated. It nicely expresses the basic algorithm of Quicksort.

```
procedure QUICKSORT(array)
    create empty arrays 'less' and 'greater' of length length('array') - 1;
    if length('array') <= 1 then
        return 'array'
    end if

    select and remove a pivot value 'pivot' from 'array';

    for each 'x' in 'array' do
        if 'x' < 'pivot' then
            append 'x' to 'less'
        else
            append 'x' to 'greater'
        end if
    end for

    return concatenate(quicksortS1('less'), 'pivot', quicksortS1('greater'));

end procedure
```

As you can see this algorithm is based on a recursive divide and conquer strategy. Notice that an implementation applying the above stated pseudo-code would consume a lot of memory. Because memory consummation is in most real life cases an important issue, the basic algorithm is usually extended in such a manner that, instead of empty space, the original array is used for storing the ‘less’ and ‘greater’ elements. This algorithm is called the “in-place” version.

The in-place version of quicksort has an extra partition phase which must ensure that the pivot is greater than all the items in the lower part and less than all those the upper part. In this manner, the original array must be rearranged before the recursion occures. In addition, the pivot can be put at its final place in the original array. In Wikipedia, you will find information about this issue as well. [Here](http://www.cs.auckland.ac.nz/~jmor159/PLDS210/qsort1a.html) is an another description of the in-place quicksort. You will see a similar version of `C` code mentioned in the above link as the starting point of the project here.

## Exercise goals

### Safely parallelize Quicksort using Cilk (5p)

The starting point is a sequential implementation of the Quicksort algorithm that has been stated as _properly working_. Usually, in serial environments, testing shows this. In parallelized environments it is hard to achieve an absolute proof with testing. Notice, for example that each recursive invocation of the function receives as parameter the same array a. In a serial implementation manipulating the same data from multiple occurrences doesn't produce any harm. If done by parallel executions, such a manipulation may be very dangerous. Thus, be careful in this respect when developing a parallelization. Because testing may not show the existence of such trouble, it is good practice to provide some proof that no harm can happen.

### Utilization analysis (5p)

Further, one design may result in only a small portion of parallelism, whereas another design achieves much more. The utilization of the available potentially parallel working processing resources (= parallelization factor) shall be as close as possible to 100% all the time. Make a parallelization analysis of your implementation. This is paper work that figures out the parallelization factor at certain phases in the execution, explains the reasons why a particular level is achieved and presuming the level is not 100% what are the reasons for missing 100%. 

### Scale-up analysis (5p)

Multi-core programming makes sense only if there is something to solve that requires massive execution. Little things are best done in the ordinary single core fashion. Therefore, make a scaling up analysis of your implementation. In case of quicksort, execute the implementation with arrays of various sizes. Try to increase the array up to the limits of your computer. Observe what happens, measure the timing. Explain the reasons for your results, in particular unexpected behavior. Make suggestions to improve the scaling up if feasible.

## Submission of report

This exercise requires a report that will be graded out of 15 points for each subsection presented above. The report shall be named as _LastName_StudentNumber_ and submitted via **Moodle**. You can use markdown, pdf or word as document formats. In addition, once your code is ready to be presented, tag the *master* branch as **final** and push the tag to the remote on **GitHub** before the deadline **24th November 2016 23:55:00**.
