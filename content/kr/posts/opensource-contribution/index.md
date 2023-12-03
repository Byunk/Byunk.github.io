+++
title = "오픈소스 컨트리뷰션"
summary = "얼마 전 오픈소스 프로젝트인 [sql-metadata](https://github.com/macbre/sql-metadata)의 컨트리뷰터가 되었다."
date = 2023-09-19T00:24:00+09:00
+++

얼마 전 오픈소스 프로젝트인 [sql-metadata](https://github.com/macbre/sql-metadata)의 컨트리뷰터가 되었다.

{{< figure src="mention-me.jpg" position="center" style="border-radius: 8px; width: 50%; margin: 0 auto;" >}}

총 두 차례의 release에 기여를 했고, 기여를 위한 기여가 아닌 core logic의 버그를 찾아내고 직접 수정한 것이기에 더 뿌듯하다.

이 오픈소스에 기여를 시작하게 된 건 우연한 기회였는데, 회사에서 새로 시작한 프로젝트에서 간단한 SQL Parser가 필요했고, 가장 가벼워 보이는 라이브러리를 선택한 것이 그 시작이었다. `sql-metadata`에 의존성을 갖는 스크립트를 짜고 TPC-DS라는 queryset을 이용해 실험을 시작했는데 이상하게 프로그램이 자꾸 터지거나 예상치 못한 결과를 내는 것이었다. 문제를 파악해보니 외부 라이브러리인 `sql-metadata`의 문제였고, 곧바로 이슈를 작성했다.

{{< figure src="issue.png" position="center" style="border-radius: 8px;" >}}

그러나 이틀이 지나도 답변이 달리지 않았고, 실험을 해야 하는 나는 두 가지 선택지 중 하나를 선택해야 했다.

1. 오픈소스 코드를 수정한다.
2. `sql-metadata`에 대한 의존성을 삭제한다.

2를 선택할 경우 필요한 작업의 양이 너무 많을 것으로 생각했고, 뚜렷한 대안도 존재하지 않았기 때문에 1을 선택했고 소스 코드를 분석하기 시작했다. 당연히 모든 코드를 순서대로 읽어보진 않았고 `pdb`를 이용해 function call이 어떤 순서로 일어나는지를 개략적으로 파악했다. 다행히 소스코드가 클린 하게 잘 짜여있었기에, 어렵지 않게 문제가 생기는 이유를 파악했고 수정할 수 있었다. 문제는 SQL의 FROM statment에서 INNER JOIN 뒤에 ','가 존재하는 경우를 배제하고 있는 것이 원인이었다. 곧바로 테스트케이스와 comment를 추가해 PR을 작성했다.

{{< figure src="PR.png" position="center" style="border-radius: 8px;" >}}

Review comment 얼마 지나지 않아 달렸고, 이 과정에서 소통이 원활히 이루어져서 PR에 한 가지의 버그 케이스를 추가했다.

{{< figure src="review1.png" position="center" style="border-radius: 8px;" >}}

추가로, linter 및 prettier에서 발생한 사소한 오류를 수정하고 PR이 merge 되었다. 이후에도 한 개의 issue를 report 하고 PR 올려서 merge 되었고, 다른 사람들의 issue도 시간 되는대로 수정해서 PR을 올리고 있다. 컨트리뷰션 중에 가장 걱정되는 점은 '내가 수정한 로직이 다른 side effect를 발생시키면 어떡하지?' 등의 우려였는데, project owner는 다른 테스트를 통과했다면 큰 문제 없을 것이라는 뉘앙스로 대수롭지 않게 여기는 것을 보고 쓸데없는 걱정이었다는 생각이 들었다.

이 글을 보고 있는 다른 사람들도, 기여를 하고 싶은 프로젝트나 혹은 이슈를 발견했다면 주저하지 않고 PR을 만들어보는 것을 권한다. 오픈 소스에 적극적으로 참여하고 건전한 생태계를 만들어나가는 것이 개발자의 기본 소양 중 하나라고 생각하며, 기여를 하기 위해 엄청난 실력을 가진 개발자가 될 필요도 없다. 단지, 프로젝트에 관심과 조금의 배경지식이면 충분하다.
