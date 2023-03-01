# Undecodable Codes

[Link](https://vjudge.net/contest/538106#problem/B)

## Description

Given $m$ 0-1 strings.

Your task is to find the least lexicographical string under the condition that its length is minimum such that it has two different partition composed of given strings.

## Input Format

$1\le m\le 20$

$1\le \text{size of each string}\le 20$

## Output Format

The satified sting.

## Solution

Creat a graph $G$.

The vertex set of $G$ is each suffix of the strings.

More specifically, node $i\times 20 + j$ indicates the suffix of the $i$-th string with size $j$.

 