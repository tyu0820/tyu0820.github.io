---
title: JAVA多个数组全排列
date: 2019-12-05 11:17:08
categories: 算法
tags: 算法
---
	import java.util.Arrays;
	import java.util.LinkedList;
	/**
	 * 多个数组全排列
	 * 思路:数字的第一位是第一个数组中的一个数，下一个数字为下一个数组中的一个数，以此类推采用递归
	 * @author Administrator
	 */
	public class TestCombine {
		private static LinkedList<String[]> getCombineList ( LinkedList<String[]> combineList, int courseCount, String[][] courseArrays, int start, int... indexs ){
	        start++;
	        if (start > courseCount - 1){
	            return null;
	        }
	        if (start == 0){
	            indexs = new int[courseArrays.length];
	        }
	        for ( indexs[start] = 0; indexs[start] < courseArrays[start].length; indexs[start]++ ){
	        	getCombineList (combineList, courseCount, courseArrays, start, indexs);
	            if (start == courseCount - 1){
	                String[] temp = new String[courseCount];
	                for ( int i = courseCount - 1; i >= 0; i-- ){
	                    temp[start - i] = courseArrays[start - i][indexs[start - i]];
	                }
	                combineList.add (temp);
	            }
	        }
	        return combineList;
	    }
	  
	    public static void main ( String[] args ){
	        String[] s1 = { "a1", "a2", "a3","a4","a5","a6","a7","a8","a9","a10" };
	        String[] s2 = { "b1", "b2", "b3","b4","b5","b6","b7","b8","b9","b10" };
	        String[] s3 = { "c1", "c2", "c3","c4","c5","c6","c7","c8","c9","c10" };
	        String[] s4 = { "d1", "d2", "d3","d4","d5","d6","d7","d8","d9","d10" };
	        String[] s5 = { "e1", "e2", "e3","e4","e5","e6","e7","e8","e9","e10" };
	        String[] s6 = { "f1", "f2", "f3","f4","f5","f6","f7","f8","f9","f10" };
	        String[] s7 = { "g1", "g2", "g3","g4","g5","g6","g7","g8","g9","g10" };
	        String[] s8 = { "h1", "h2", "h3","h4","h5","h6","h7","h8","h9","h10" };
	        String[] s9 = { "i1", "i2", "i3","i4","i5","i6","i7","i8","i9","i10" };
	        String[] s10 = { "j1", "j2", "j3","j4","j5","j6","j7","j8","j9","j10" };
	        String[] s11 = { "k1", "k2", "k3","k4","k5","k6","k7","k8","k9","k10" };
	        String[][] courseArrays = { s1, s2, s3};
	          
	        LinkedList<String[]> combineList = new LinkedList<String[]> ();
	        
	        getCombineList(combineList, courseArrays.length, courseArrays, -1);
	        
	        for ( int i = 0; i < combineList.size (); i++ ){
	            System.out.println (Arrays.toString (combineList.get (i)));
	        }
	        long num = combineList.size();
	        System.out.println("组合数:"+num);
	    }
	}
