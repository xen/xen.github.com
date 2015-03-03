---
layout: post
title: "My codility test answers (python)"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Recently, during interview to some company, I got tested by Codility service. And here is my performance results. Generally this is copy&paste from their email. Hope this can be helpful for others who want to see what this tests are. 

This is overal performance table: 

<table class="table table-bordered"><tbody><tr><td></td><td><b>Task name</b></td><td><b>Correctness</b></td><td><b>Performance</b></td><td><b>Task score</b></td></tr><tr><td><b>1</b></td><td>PtrListLen</td><td>100 </td><td>not assessed </td><td>100 </td></tr><tr><td><b>2</b></td><td>BugfixingLeaderSorted</td><td>100 </td><td>not assessed </td><td>100 </td></tr><tr><td><b>3</b></td><td>DeepestPit</td><td>55 </td><td>66 </td><td>60 </td></tr><tr><td><b>4</b></td><td>CountMultiplicativePairs</td><td>80 </td><td>87 </td><td>83 </td></tr><tr><td></td><td><b>Total</b></td><td><b>84 </b></td><td><b>N/A </b></td><td><b>343 / 400</b></td></tr></tbody></table>

<h2>PtrListLen - problem description</h2>

<p>A pointer is called a <i>linked list</i> if:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; it is an empty pointer (it is then called a <i>terminator</i> or an <i>empty list</i>); or</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; it points to a structure (called a <i>node</i> or the <i>head</i>) that contains a value and a linked list (called the <i>tail</i>).</p></blockquote><p>The <i>length</i> of a list is defined as the total number of nodes it contains. In particular, an empty list has length 0.</p><p>For example, consider the following linked list:</p><pre><tt>&nbsp; A -&gt; B -&gt; C -&gt; D -&gt;</tt></pre><p>This list contains four nodes: A, B, C and D. Node D is the last node and its tail is the terminator. The length of this list is 4.</p><p>Assume that the following declarations are given:</p><blockquote><p><tt>class&nbsp;IntList(object):</tt><br /><tt>&nbsp;&nbsp;value&nbsp;=&nbsp;0</tt><br /><tt>&nbsp;&nbsp;next&nbsp;=&nbsp;None </tt></p></blockquote><p>Write a function:</p><blockquote><p><tt>def solution(L) </tt></p></blockquote><p>that, given a non-empty linked list L consisting of N nodes, returns its length.</p><p>For example, given list L shown in the example above, the function should return 4.</p><p>Assume that:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N is an integer within the range [1..5,000];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; list L does not have a cycle (each non-empty pointer points to a different structure).</p></blockquote><p>In your solution, focus on <b>correctness</b>. The performance of your solution will not be the focus of the assessment.</p><p>Copyright 2009–2015 by Codility Limited. All Rights Reserved. Unauthorized copying, publication or disclosure prohibited. </p>

<h3>Tests</h3>

<p>Score: <strong>100 of 100</strong> </p><p>Estimated time complexity: <strong>None</strong> </p>
<table  class="table table-bordered"><tbody><tr><td><p><b>Test name</b></p></td><td><p ><b>Running time</b></p></td><td><p ><b>Result</b></p></td></tr><tr><td><p>example <br />example, length=4 </p></td><td><p align="right">0.060&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>extreme_single_double <br />length=1 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>three_elems <br />length=3 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>twenty_elements <br />length=20 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium <br />length=93 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium2 <br />length=999 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>1k_elements <br />length=1,000 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>quite_long <br />length=4,000 </p></td><td><p align="right">0.052&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>long <br />length=5,000 </p></td><td><p align="right">0.056&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr></tbody></table>

<h3>Solution (language: Python)</h3>

<pre># you can use print for debugging purposes, e.g.
# print "this is a debug message"
&nbsp;
def solution(L):
&nbsp;&nbsp;&nbsp; # write your code in Python 2.7
&nbsp;&nbsp;&nbsp; count = 0
&nbsp;&nbsp;&nbsp; while L:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; count = count + 1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; L = L.next
&nbsp;&nbsp;&nbsp; return count</pre>
<hr size="2" width="100%"  />

<h2>BugfixingLeaderSorted - problem description</h2>

<p>A non-empty zero-indexed array A consisting of N integers and sorted in a non-decreasing order is given. The <i>leader</i> of this array is the value that occurs in more than half of the elements of A.</p><p>You are given an implementation of a function:</p><blockquote><p><tt>def solution(A) </tt></p></blockquote><p>that, given a non-empty zero-indexed array A consisting of N integers, sorted in a non-decreasing order, returns the leader of array A. The function should return −1 if array A does not contain a leader.</p><p>For example, given array A consisting of ten elements such that:</p><pre><tt>&nbsp; A[0] = 2</tt>
<tt>&nbsp; A[1] = 2</tt>
<tt>&nbsp; A[2] = 2</tt>
<tt>&nbsp; A[3] = 2</tt>
<tt>&nbsp; A[4] = 2</tt>
<tt>&nbsp; A[5] = 3</tt>
<tt>&nbsp; A[6] = 4</tt>
<tt>&nbsp; A[7] = 4</tt>
<tt>&nbsp; A[8] = 4</tt>
<tt>&nbsp; A[9] = 6</tt></pre><p>the function should return −1, because the value that occurs most frequently in the array, 2, occurs five times, and 5 is not more than half of 10.</p><p>Given array A consisting of five elements such that:</p><pre><tt>&nbsp; A[0] = 1</tt>
<tt>&nbsp; A[1] = 1</tt>
<tt>&nbsp; A[2] = 1</tt>
<tt>&nbsp; A[3] = 1</tt>
<tt>&nbsp; A[4] = 50</tt></pre>
<p>the function should return 1.</p><p>Unfortunately, despite the fact that the function may return expected result for the example input, there is a bug in the implementation, which may produce incorrect results for other inputs. Find the bug and correct it. You should modify at most <b>three</b> lines of code.</p><p>Assume that:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N is an integer within the range [1..100,000];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; each element of array A is an integer within the range [0..2,147,483,647];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; array A is sorted in non-decreasing order.</p></blockquote><p>Complexity:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case time complexity is O(N);</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).</p></blockquote><p>Elements of input arrays can be modified.</p><p>Copyright 2009–2015 by Codility Limited. All Rights Reserved. Unauthorized copying, publication or disclosure prohibited. </p>

<h3>Tests</h3>
<p>Score: <strong>100 of 100</strong> </p><p>Estimated time complexity: <strong>None</strong> </p>

<table class="table table-bordered"><tbody><tr><td><p ><b>Test name</b></p></td><td><p ><b>Running time</b></p></td><td><p ><b>Result</b></p></td></tr><tr><td><p>example1 <br />first example test </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>example2 <br />second example test </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>simple1 <br />values from a continuous range </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>simple2 <br />0s/1s only </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>single <br />one element </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>two_values <br />two different values </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>extreme_big_values <br />min/max values only </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium_1 <br />small sequence repeated many times </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium_2 <br />no leader and small sequence with values from a continuous range </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>cyclic_sequence <br />no leader and small sequence repeated many times </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium_random <br />random sequences </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>large <br />two different values, length = ~100,000 </p></td><td><p align="right">0.104&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>large_range <br />values from a continuous range, length = ~100,000 </p></td><td><p align="right">0.108&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr></tbody></table>

<h3>Solution (language: Python)</h3>

<pre>def solution(A): 
&nbsp;&nbsp;&nbsp;&nbsp;n = len(A)
&nbsp;&nbsp;&nbsp; L = [-1] + A
&nbsp;&nbsp;&nbsp; count = 0
&nbsp;&nbsp;&nbsp; pos = (n + 1) // 2
&nbsp;&nbsp;&nbsp; candidate = L[pos]
&nbsp;&nbsp;&nbsp; for i in xrange(1, n + 1):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (L[i] == candidate):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; count = count + 1
&nbsp;&nbsp;&nbsp; if (2*count &gt; n):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return candidate
&nbsp;&nbsp;&nbsp; return -1</pre>

<hr size="2" width="100%"  />

<h2>DeepestPit - problem description</h2>

<p>A non-empty zero-indexed array A consisting of N integers is given. A <i>pit</i> in this array is any triplet of integers (P, Q, R) such that:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0 ≤ P &lt; Q &lt; R &lt; N;</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sequence [A[P], A[P+1], ..., A[Q]] is strictly decreasing, <br />i.e. A[P] &gt; A[P+1] &gt; ... &gt; A[Q];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sequence A[Q], A[Q+1], ..., A[R] is strictly increasing, <br />i.e. A[Q] &lt; A[Q+1] &lt; ... &lt; A[R].</p></blockquote><p>The <i>depth</i> of a pit (P, Q, R) is the number <b>min</b>{A[P] − A[Q], A[R] − A[Q]}.</p><p>For example, consider array A consisting of 10 elements such that:</p><pre><tt>&nbsp; A[0] =&nbsp; 0</tt>
<tt>&nbsp; A[1] =&nbsp; 1</tt>
<tt>&nbsp; A[2] =&nbsp; 3</tt>
<tt>&nbsp; A[3] = -2</tt>
<tt>&nbsp; A[4] =&nbsp; 0</tt>
<tt>&nbsp; A[5] =&nbsp; 1</tt>
<tt>&nbsp; A[6] =&nbsp; 0</tt>
<tt>&nbsp; A[7] = -3</tt>
<tt>&nbsp; A[8] =&nbsp; 2</tt>
<tt>&nbsp; A[9] =&nbsp; 3</tt></pre><p>Triplet (2, 3, 4) is one of pits in this array, because sequence [A[2], A[3]] is strictly decreasing (3 &gt; −2) and sequence [A[3], A[4]] is strictly increasing (−2 &lt; 0). Its depth is <b>min</b>{A[2] − A[3], A[4] − A[3]} = 2. Triplet (2, 3, 5) is another pit with depth 3. Triplet (5, 7, 8) is yet another pit with depth 4. There is no pit in this array deeper (i.e. having depth greater) than 4.</p><p>Write a function:</p><blockquote><p><tt>def solution(A) </tt></p></blockquote><p>that, given a non-empty zero-indexed array A consisting of N integers, returns the depth of the deepest pit in array A. The function should return −1 if there are no pits in array A.</p><p>For example, consider array A consisting of 10 elements such that:</p><pre><tt>&nbsp; A[0] =&nbsp; 0</tt>
<tt>&nbsp; A[1] =&nbsp; 1</tt>
<tt>&nbsp; A[2] =&nbsp; 3</tt>
<tt>&nbsp; A[3] = -2</tt>
<tt>&nbsp; A[4] =&nbsp; 0</tt>
<tt>&nbsp; A[5] =&nbsp; 1</tt>
<tt>&nbsp; A[6] =&nbsp; 0</tt>
<tt>&nbsp; A[7] = -3</tt>
<tt>&nbsp; A[8] =&nbsp; 2</tt>
<tt>&nbsp; A[9] =&nbsp; 3</tt></pre><p>the function should return 4, as explained above.</p><p>Assume that:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N is an integer within the range [1..1,000,000];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; each element of array A is an integer within the range [−100,000,000..100,000,000].</p></blockquote><p>Complexity:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case time complexity is O(N);</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).</p></blockquote><p>Elements of input arrays can be modified.</p><p>Copyright 2009–2015 by Codility Limited. All Rights Reserved. Unauthorized copying, publication or disclosure prohibited. </p>

<h3>Tests</h3>

<p>Score: <strong>60 of 100</strong> </p>

<table class="table table-bordered"><tbody><tr><td><p ><b>Test name</b></p></td><td><p ><b>Running time</b></p></td><td><p ><b>Result</b></p></td></tr><tr><td><p>example <br />example test </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>extreme_no_pit <br />small test cases </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got -1 expected 1 </p></td></tr><tr><td><p>extreme_depth </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got -1 expected 200000000 </p></td></tr><tr><td><p>simple1 <br />no pit </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got -1 expected 1 </p></td></tr><tr><td><p>simple2 <br />one pit </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got -1 expected 50 </p></td></tr><tr><td><p>user <br />user-defined test case </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>simple3 <br />`vulcano' shape </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>retries <br />retries </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium1 <br />medium correctness test </p></td><td><p align="right">0.104&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>medium_pit <br />medium test one pit </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>large_pit_1 <br />large test one pit 1 </p></td><td><p align="right">0.148&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got 43265 expected 43287 </p></td></tr><tr><td><p>large_pit_2 <br />large test one pit 2 </p></td><td><p align="right">0.152&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got 432950 expected 433170 </p></td></tr><tr><td><p>big_pit_1 <br />big test one pit 1 </p></td><td><p align="right">0.120&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>big_pit_2 <br />big test one pit 1 </p></td><td><p align="right">0.132&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>big3_1 <br />large random test </p></td><td><p align="right">0.424&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>big3_2 <br />big random test </p></td><td><p align="right">1.336&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr></tbody></table>

<h3>Solution (language: Python)</h3>

<pre># you can use print for debugging purposes, e.g.
# print "this is a debug message"
&nbsp;
def solution(A):
&nbsp;&nbsp;&nbsp; deepest = 0
&nbsp;&nbsp;&nbsp; pit = lambda p, q, r: min(A[p] - A[q], A[r] - A[q])
&nbsp;&nbsp;&nbsp; p = 0
&nbsp;&nbsp;&nbsp; r = -1
&nbsp;&nbsp;&nbsp; q = -1
&nbsp;&nbsp;&nbsp; l = len(A)
&nbsp;&nbsp;&nbsp; for i in xrange(0, l):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if q&lt;0 and A[i]&gt;=A[i-1]:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; q = i -1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (q&gt;=0 and r&lt;0) and (A[i]&lt;=A[i-1] or i+1==l):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; r = i-1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; deepest = max(deepest, pit(p, q, r))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; p = i-1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; q = -1 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;r = -1
&nbsp;&nbsp;&nbsp; return deepest if deepest else -1</pre>

<hr size="2" width="100%"  />

<h2>CountMultiplicativePairs - problem description</h2>
<p>Zero-indexed arrays A and B consisting of N non-negative integers are given. Together, they represent N real numbers, denoted as C[0], ..., C[N−1]. Elements of A represent the integer parts and the corresponding elements of B (divided by 1,000,000) represent the fractional parts of the elements of C. More formally, A[I] and B[I] represent C[I] = A[I] + B[I] / 1,000,000.</p><p>For example, consider the following arrays A and B:</p><pre><tt>&nbsp; A[0] = 0&nbsp;&nbsp;&nbsp; B[0] = 500,000</tt>
<tt>&nbsp; A[1] = 1&nbsp;&nbsp;&nbsp; B[1] = 500,000</tt>
<tt>&nbsp; A[2] = 2&nbsp;&nbsp;&nbsp; B[2] = 0</tt>
<tt>&nbsp; A[3] = 2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; B[3] = 0</tt>
<tt>&nbsp; A[4] = 3&nbsp;&nbsp;&nbsp; B[4] = 0</tt>
<tt>&nbsp; A[5] = 5&nbsp;&nbsp;&nbsp; B[5] = 20,000</tt></pre><p>They represent the following real numbers:</p><pre><tt>&nbsp; C[0] = 0.5</tt>
<tt>&nbsp; C[1] = 1.5</tt>
<tt>&nbsp; C[2] = 2.0</tt>
<tt>&nbsp; C[3] = 2.0</tt>
<tt>&nbsp; C[4] = 3.0</tt>
<tt>&nbsp; C[5] = 5.02</tt></pre><p>A pair of indices (P, Q) is <i>multiplicative</i> if 0 ≤ P &lt; Q &lt; N and C[P] * C[Q] ≥ C[P] + C[Q].</p><p>The above arrays yield the following multiplicative pairs:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (1, 4), because 1.5 * 3.0 = 4.5 ≥ 4.5 = 1.5 + 3.0,</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (1, 5), because 1.5 * 5.02 = 7.53 ≥ 6.52 = 1.5 + 5.02,</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (2, 3), because 2.0 * 2.0 = 4.0 ≥ 4.0 = 2.0 + 2.0,</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (2, 4) and (3, 4), because 2.0 * 3.0 = 6.0 ≥ 5.0 = 2.0 + 3.0.</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (2, 5) and (3, 5), because 2.0 * 5.02 = 10.04 ≥ 7.02 = 2.0 + 5.02.</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (4, 5), because 3.0 * 5.02 = 15.06 ≥ 8.02 = 3.0 + 5.02.</p></blockquote><p>Write a function:</p><blockquote><p><tt>def solution(A, B) </tt></p></blockquote><p>that, given zero-indexed arrays A and B, each containing N non-negative integers, returns the number of multiplicative pairs of indices.</p><p>If the number of multiplicative pairs is greater than 1,000,000,000, the function should return 1,000,000,000.</p><p>For example, given:</p><pre><tt>&nbsp; A[0] = 0&nbsp;&nbsp;&nbsp; B[0] = 500,000</tt>
<tt>&nbsp; A[1] = 1&nbsp;&nbsp;&nbsp; B[1] = 500,000</tt>
<tt>&nbsp; A[2] = 2&nbsp;&nbsp;&nbsp; B[2] = 0</tt>
<tt>&nbsp; A[3] = 2&nbsp;&nbsp;&nbsp; B[3] = 0</tt>
<tt>&nbsp; A[4] = 3&nbsp;&nbsp;&nbsp; B[4] = 0</tt>
<tt>&nbsp; A[5] = 5&nbsp;&nbsp;&nbsp; B[5] = 20,000</tt></pre><p>the function should return 8, as explained above.</p><p>Assume that:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N is an integer within the range [0..100,000];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; each element of array A is an integer within the range [0..1,000];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; each element of array B is an integer within the range [0..999,999];</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; real numbers created from arrays are sorted in non-decreasing order.</p></blockquote><p>Complexity:</p><blockquote><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case time complexity is O(N);</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expected worst-case space complexity is O(1), beyond input storage (not counting the storage required for input arguments).</p></blockquote><p>Elements of input arrays can be modified.</p><p>Copyright 2009–2015 by Codility Limited. All Rights Reserved. Unauthorized copying, publication or disclosure prohibited. </p>

<h3>Tests</h3>

<p>Score: <strong>83 of 100</strong> </p><p>Estimated time complexity: <strong>O(N)</strong> </p>

<table class="table table-bordered"><tbody><tr><td><p ><b>Test name</b></p></td><td><p ><b>Running time</b></p></td><td><p ><b>Result</b></p></td></tr><tr><td><p>example <br />example test </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>simple_diagonal <br />simple test with A = B </p></td><td><p align="right">0.052&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>simple_equality <br />simple test with multiple equalities to be counted </p></td><td><p align="right">0.052&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>mulitple_zeros <br />many zeros </p></td><td><p align="right">0.052&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got 1 expected 2 </p></td></tr><tr><td><p>extreme_empty <br />empty sequence + [1.5, 3.01] </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got -1 expected 0 </p></td></tr><tr><td><p>extreme_single <br />singleton sequence + [1.4, 3.5] </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>doubles <br />2-element sequences, precise calculation </p></td><td><p align="right">0.040&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>precision_small <br />precise calculation, N = 400 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>geometric_small <br />geometric sequence, N = 111 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>arithmetic_small <br />arithmetic sequence, N = ~300 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>random_small <br />random sequence, N = ~600 </p></td><td><p align="right">0.044&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>geometric_medium <br />geometric sequence, N = ~9,000 </p></td><td><p align="right">0.064&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>random_medium <br />random sequence, N = ~10,000 </p></td><td><p align="right">0.064&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>arithmetic_medium <br />arithmetic sequence, N = 20,000 </p></td><td><p align="right">0.088&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>geometric_large <br />geometric sequence, N = ~60,000 </p></td><td><p align="right">0.188&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>arithmetic_large <br />arithmetic sequence, N = 90,000 </p></td><td><p align="right">0.244&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>random_large <br />random sequence, N = 100,000 </p></td><td><p align="right">0.272&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>extreme_large <br />big numbers, N = 100,000 </p></td><td><p align="right">0.280&nbsp;s </p></td><td><p><b>OK</b> </p></td></tr><tr><td><p>extreme_zeros <br />almost all zeros, N &lt;= 100,000 </p></td><td><p align="right">0.224&nbsp;s </p></td><td><p><b>WRONG ANSWER</b> <br />got 0 expected 1000000000 </p></td></tr></tbody></table>

<h3>Solution (language: Python)</h3>

<pre>
def solution(A, B):
&nbsp;&nbsp;&nbsp; # write your code in Python 2.7
&nbsp;&nbsp;&nbsp; if not len(A) or not len(B) or len(A) != len(B):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return -1
&nbsp;
&nbsp;&nbsp;&nbsp; # make C and filter all values &lt;= 1
&nbsp;&nbsp;&nbsp; C = [A[i]+float(B[i])/1000000 for i in range(len(A)) if A[i]+float(B[i])/1000000 &gt; 1]
&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;C.sort()
&nbsp;&nbsp;&nbsp; result = 0
&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;p = 0&nbsp; # position
&nbsp;&nbsp;&nbsp; l = len(C) - 1
&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;while l &gt; p:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; res = C[l] / (C[l] - 1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; while (p &lt; l and C[p] &lt; res):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; p = p + 1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if p == l:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; break
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; result = result + (l-p)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if result &gt; 1000000000:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return 1000000000
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;l = l-1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
&nbsp;
&nbsp;&nbsp;&nbsp; return result</pre>
