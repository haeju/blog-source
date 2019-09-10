---
title: d3 간단 소개
date: 2019-08-23 20:17:29
categories:
  - front-end
  - d3
tags:
  - d3
---
# d3 간단 소개
<!-- more -->
- ## SVG 구성요소
    1. ***`<svg>`***
    2. ***`<circle>, <rect>, <line>, <polygon>`***
    3. ***`<text>`***
    4. ***`<g>`***
    5. ***`<path>`***
        - d 속성이 결정하는 도형의 영역이다. 
        - d 속성 마지막에 Z 문자열이 있으면 영역(area)이되고 없다면 선이나 곡선등 열린 도형(?)이 된다.

- ## 데이타 변환
    1. ***캐스팅 ( 형변환 )***
    2. ***정규화 ( 스케일과 규모변경 )***
        - scaleLinear()
    3. ***비닝 ( 그룹화 : 데이터 분류 )***
    4. ***내포 ( 계층화 )***


- ## d3가 제공하는 기능의 3대 유형
    1. ***생성기***
        - `<path>`요소의 d 속성을 작성하는데 필요한 과정 추상화함으로써 복잡한 SVG `<path>`요소의 생성과정을 단순하게 만들어준다.
        - `<path>`요소에 들어가는 d 속성 문자열을 생성
        - d3.svg.line(), d3.svg.area(), d3.svg.arc(), d3.svg.diagoal() 
    2. ***컴포넌트***
        - 특정 차트 컴포넌트를 그리는데 필요한 일련의 화면 객체 생성
        - d3.svg.axis : `<line>`, `<path>`, `<g>`, `<text>`요소를 생성
        ```js
        import * as d3 from 'd3'
        var scatterData = [{ friends: 5, salary: 22000 },
        { friends: 3, salary: 18000 }, { friends: 10, salary: 88000 },
        { friends: 0, salary: 180000 }, { friends: 27, salary: 56000 },
        { friends: 8, salary: 74000 }]

        function test(instance) {
            const targetContainer = instance.$el
            d3.select(targetContainer).append('svg')
            draw()    
        }

        function draw(){
            var xExtent = d3.extent(scatterData, d=> d.salary)
            var yExtent = d3.extent(scatterData, d=> d.friends)
            
            var xScale = d3.scaleLinear().domain(xExtent).range([0,500])
            var yScale = d3.scaleLinear().domain(yExtent).range([0,500])

            d3.select('svg').selectAll('circle')
                .data(scatterData).enter().append('circle')
                .attr('r', 5).attr('cx', d => xScale(d.salary))
                .attr('cy', d => yScale(d.friends))

            var yAxis = d3.axisRight().scale(yScale)
            d3.select('svg').append('g').attr('id','yAxisG').call(yAxis)

            var xAxis = d3.axisBottom().scale(xScale)
            d3.select('svg').append('g').attr('id','xAxisG').call(xAxis)
        }

        export {
            test
        }
        ```
        - 점으로 선 그리기
        ```js
        import * as d3 from 'd3'

        var scatterData = [{ day: 1, friends: 5, salary: 3 },
        { day: 2, friends: 3, salary: 4 }, { day: 3, friends: 10, salary: 7 },
        { day: 4, friends: 0, salary: 7 }, { day: 5, friends: 27, salary: 11 },
        { day: 6, friends: 8, salary: 1 }]

        function test(instance) {
            const targetContainer = instance.$el
            d3.select(targetContainer).append('svg')
            draw()
        }

        function draw() {
            const blue = '#5eaec5', green = '#92c463'
            var xScale = d3.scaleLinear().domain([0, 10]).range([0, 500])
            var yScale = d3.scaleLinear().domain([0, 27]).range([0, 500])
            var xAxis = d3.axisBottom()
                .scale(xScale)
                .tickSize(500)
                .ticks(4)

            d3.select('svg').append('g').attr('id', 'xAxisG').call(xAxis)
            var yAxis = d3.axisRight()
                .scale(yScale)
                .ticks(16)
                .tickSize(500)
            d3.select('svg').append('g').attr('id', 'yAxisG').call(yAxis)

            d3.select('svg').selectAll('circle.friends')
                .data(scatterData)
                .enter()
                .append('circle')
                .attr('class', 'friends')
                .attr('r', 5)
                .attr('cx', d => xScale(d.day))
                .attr('cy', d => yScale(d.friends))
                .style('fill', blue)

            d3.select('svg').selectAll('circle.salary')
                .data(scatterData)
                .enter()
                .append('circle')
                .attr('class', 'salary')
                .attr('r', 5)
                .attr('cx', d => xScale(d.day))
                .attr('cy', d => yScale(d.salary))
                .style('fill', green)

            // SVG 그림 코드 반환    
            var tweetLineFriends = d3.line()
                .x(d => xScale(d.day))
                .y(d => yScale(d.friends))
            //선 보간법 설정
            tweetLineFriends.curve(d3.curveCardinal)        
            d3.select('svg')
                .append('path')
                .attr('d', tweetLineFriends(scatterData))
                .attr('fill', 'none')
                .attr('stroke', 'darkred')
                .attr('stroke-width', 2)
            
            // SVG 그림 코드 반환    
            var tweetLineSalary = d3.line()
                .x(d => xScale(d.day))
                .y(d => yScale(d.salary))
            //선 보간법 설정
            tweetLineSalary.curve(d3.curveStep)        
            // tweetLineSalary.curve(d3.curveBasis)                
            d3.select('svg')
                .append('path')
                .attr('d', tweetLineSalary(scatterData))
                .attr('fill', 'none')
                .attr('stroke', 'gray')
                .attr('stroke-width', 2)    
        }
        ```
        - 누적 차트
        ```js
        import * as d3 from 'd3'

        function test(instance) {
            const targetContainer = instance.$el
            d3.select(targetContainer).append('svg')
            draw()

        }

        // movie.csv
        // day,titanic,avatar,akira,frozen,deliverance,avengers
        // 1,20,8,3,0,0,0
        // 2,18,5,1,13,0,0
        // 3,14,3,1,10,0,0
        // 4,7,3,0,5,27,15
        // 5,4,3,0,2,20,14
        // 6,3,1,0,0,10,13
        // 7,2,0,0,0,8,12
        // 8,0,0,0,0,6,11
        // 9,0,0,0,0,3,9
        // 10,0,0,0,0,1,8


        function draw() {
            d3.csv('/src/renderer/pages/countryInfo/d3Sample/4/movie.csv').then(data => areaChart(data))
        }

        function areaChart(data) {
            var fillScale = d3.scaleOrdinal()
                .domain(['titanic', 'avatar', 'akira', 'frozen', 'deliverance', 'avengers'])
                .range(['#fcd88a', '#cf7c1c', '#93c464', '#75734F', '#5eafc6', '#41a368'])

            var xScale = d3.scaleLinear().domain([1, 8]).range([20, 470])
            var yScale = d3.scaleLinear().domain([0, 55]).range([480, 20])

            Object.keys(data[0]).forEach(key => {
                if (key != 'day') {
                    var movieArea = d3.area()
                        .x(d => xScale(d.day))
                        .y0(d => yScale(simpleStacking(d, key) - d[key]))
                        .y1(d => yScale(simpleStacking(d, key)))
                        .curve(d3.curveBasis)

                    d3.select('svg')
                        .append('path')
                        .style('id', key + 'Area')
                        .attr('d', movieArea(data))
                        .attr('fill', fillScale(key))
                        .attr('stroke', 'black')
                        .attr('stroke-width', 1)
                }
            })
            function simpleStacking(lineData, lineKey) {
                var newHeight = 0
                Object.keys(lineData).every(key => {
                    if (key !== 'day') {
                        newHeight += parseInt(lineData[key])
                        if (key === lineKey) {
                            return false
                        }
                    }
                    return true
                })
                return newHeight
            }
            var legendA = d3.legendColor().scale(fillScale)
            d3.select('svg')
                .style('width', '1000px')

            d3.select('svg')
                .append('g')
                .attr('transform', 'translate(500, 0)')
                .call(legendA)
        }
        ```

        - d3.svg.brush
    3. ***레이아웃***
        - 일련의 데이터, 그리고 생성기로 구성된 배열을 입력받아 특정 위치와 크기로 그리는데 필요한 데이터 속성을 동적,정적 추가

