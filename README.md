# CILK Exercise - Quicksort

## Getting started with exercise

1. The starting point of this exercise is the git repository you imported/created from the instructions on moodle. The repository will have the structure `quicksort-yourgithubid`. For the sake of this readme, lets call it `quicksort-skanur`.

2. Open a terminal and clone your repository on to your local machine using `git clone`. `cd` to the new directory created. This is your project directory and every step assumes you are in this directory. 
    ```bash
    git clone https://github.com/ESLab/quicksort-skanur.git
    cd quicksort-skanur
    ```

2. Load the compiler and related libraries. Note that there is no spaces between `CC=icc` and `CXX=icpc`
    ```bash
    use parallelstudio
    export CC=icc CXX=icpc
    ```

3. Navigate to the assignment directory and create a new folder `build` and navigate into it i.e.
    ```bash
    mkdir -p build
    cd build
    ```

4. To compile the code and run, from project directory 
    ```bash
    cd build
    cmake ..
    make
    ./qsort
    ```

5. To clean the project, just delete `build` directory and create a new one i.e. from your project directory
    ```bash
    rm -rf build
    mkdir -p build
    ```

6. You can see the utilization of the CPUs using the command `htop` or `top`. Open another terminal and run either of these commands. You can also measure system usage during the code execution using the script `measure.sh`. Execute this script by using appropriate compiler environment and navigating back to your project folder and running
    ```bash
    ./measure.sh
    ```
    This will print out CPU utilization all the cores every second.

7. Work on the code using your favorite text editor repeating steps 3-6 to clean, build and execute application. Make local commits on points you think are important (for e.g. various parallelisation strategies). Push it to remote when you think necessary. 

8. Once completed with the assignment, create a tag **final** for your master branch using `git tag -a final -m "Assignment complete"`.

9. Push the repository to remote using `git push --follow-tags -u origin master` before the exercise deadline of **24th November 2016 23:55:00**.

10. **WARNING**: This repo will be deleted after the exercise assessment without any prior notice. If you want a copy of it, fork it **privately**.

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

Hint: Go through the descriptions of the standard functions used in the code (e.g. std::partition) to motivate why data race might or might not happen in practice.

### Utilization analysis (5p)

Further, one design may result in only a small portion of parallelism, whereas another design achieves much more. The utilization of the available potentially parallel working processing resources (= parallelization factor) shall be as close as possible to 100% all the time. Make a parallelization analysis of your implementation. There can be alternative ways of parallelizing the same application based on which recursion is used for spawning tasks. Try atleast two such possibilities and explain the reasons why a particular level of parallelization is achieved and presuming the level is not 100% what are the reasons for missing 100%.

### Scale-up analysis (5p)

Multi-core programming makes sense only if there is something to solve that requires massive execution. Little things are best done in the ordinary single core fashion. Therefore, make a scaling up analysis of your implementation. In case of quicksort, execute the implementation with arrays of various sizes. Try to increase the array up to the limits of your computer. Observe what happens, measure the timing. Explain the reasons for your results, in particular unexpected behavior. Make suggestions to improve the scaling up if feasible.

## Submission of report

This exercise requires a report that will be graded out of 15 points for each subsection presented above. The report shall be named as _LastName_StudentNumber_ and submitted via **Moodle**. You can use markdown, pdf or word as document formats. In addition, once your code is ready to be presented, tag the *master* branch as **final** and push the tag to the remote on **GitHub** before the deadline **24th November 2016 23:55:00**.
