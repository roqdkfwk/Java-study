# Facade Pattern

---

## Facade 패턴 개요

Facade 패턴은 객체 지향 설계 패턴 중 하나로, 시스템의 복잡한 **서브시스템을 단순하게 사용할 수 있도록 통합된 인터페이스를 제공하는 역할**을 한다. 이 패턴은 클라이언트가 서브시스템의 세부 사항을 알 필요 없이, 단순한 인터페이스를 통해 다양한 기능을 쉽게 사용할 수 있게 한다.

`Controller` → `Facade Class` → `Service` → `Repository` 순서로 구성할 수 있으며
`Controller` → `Service` → `Facade Class` → `Repository` 순서로 구성할 수도 있다.

위의 경우, `Contoller`가 클라이언트, `Service`가 서브시스템이며 아래의 경우 `Service`가 클라이언트, `Repository`가 서브시스템이다.

![image.png](./images/Facade%20Pattern.png)

---

## 구성 요소

### Facade

**클라이언트가 사용하는 간단한 인터페이스를 제공**한다. 이 인터페이스는 복잡한 서브시스템의 기능들을 통합하여 제공하며, 클라이언트는 이를 통해 서브시스템의 복잡성을 알지 않고도 필요한 작업을 수행할 수 있다.

---

### Subsystem classes

**실제로 복잡한 작업을 수행하는 클래스**이다. Facade 클래스는 이러한 서브시스템 클래스들과 상호작용하며, 클라이언트에게 단순한 인터페이스를 제공하고 내부적으로 복잡한 로직을 처리한다.

---

### Client

Facade 객체를 사용하여 시스템의 기능을 활용하는 주체이다. 클라이언트는 서브시스템의 세부 사항에 대해 신경 쓰지 않고, Facade가 제공하는 단순한 인터페이스를 통해 기능을 사용한다.

---

## Facade 패턴의 장점

### 복잡성 감소

서브시스템의 복잡한 구조를 숨기고 단순한 인터페이스를 제공함으로써, 시스템을 쉽게 사용할 수 있다.

---

### 서브시스템과의 결합도 감소

클라이언트와 서브시스템 간의 결합도가 줄어들어, 시스템을 더 유연하게 만들고 쉽게 확장 가능하게 만든다. 클라이언트는 Facade를 통해 서브시스템과 상호작용하므로, 서브시스템의 변경이 클라이언트에 영향을 미치지 않는다.

---

### 코드 유지보수성 향상

클라이언트 코드가 간단해지고, 서브시스템에 대한 직접적인 참조가 줄어들어, 코드 유지보수가 용이해진다.

---

### 순환참조 오류 해결

두 개의 클래스가 서로를 참조해야 하는 상황에서, 두 클래스가 서로를 직접 참조하는 대신 Facade를 통해 상호작용하게 만들면, 순환 참조를 피할 수 있다.

---

## Facade 패턴의 단점

### Facade의 비대화

시스템이 커지거나 복잡해지면 Facade 클래스가 너무 많은 기능을 포함하게 되어 비대해질 수 있다. 이로 인해 Facade 자체가 복잡해질 수 있으며, Facade의 유지 보수성이 떨어질 수 있다.

---

### 성능 저하

Facade 패턴은 추가적인 추상화 계층을 도입한다. 이로 인해 시스템이 복잡해지거나 불필요한 메서드 호출이 증가해 성능이 저하될 수 있다.

---

### 서브시스템의 확장성 제한

Facade 패턴은 서브시스템을 단순화된 인터페이스로 감싸므로, 서브시스템을 확장하거나 변경할 때 Facade 자체도 수정해야 할 가능성이 발생한다. 이로 인해 서브시스템의 확장성이 제한될 수 있다.

---

### 실제 적용

- `BasicService`
  ```java
  @Service
  @RequiredArgsConstructor
  @Transactional
  public class BasicService {

      private final CourseRepository courseRepository;
      private final CurriculumRepository curriculumRepository;
      private final com.ssafy.db.repository.FavorRepository favorRepository;
      private final InstructorRepository instructorRepository;
      private final MemberRepository memberRepository;
      private final RegisterRepository registerRepository;
      private final ReviewRepository reviewRepository;
      private final TagRepository tagRepository;

      // RegisterRepository
      public void saveRegister(Register register) {
          registerRepository.save(register);
      }

      public Boolean existsRegisterByMemberIdAndCourseId(Long memberId, Long courseId) {
          return registerRepository.existsRegisterByMemberIdAndCourseId(memberId, courseId) != null;
      }

      // CourseRepository
      @Transactional(readOnly = true)
      public Course findCourseByCourseId(Long courseId) {
          return courseRepository.findById(courseId).orElseThrow(
                  () -> new NotFoundException("Not Found Course : CourseId is " + courseId)
          );
      }

      public void saveCourse(Course course) {
          courseRepository.save(course);
      }

      // CurriculumRepository
      @Transactional(readOnly = true)
      public Curriculum findCurriculumByCurriculumId(Long curriculumId) {
          return curriculumRepository.findById(curriculumId).orElseThrow(
                  () -> new NotFoundException("Not Found Curriculum : CurriculumId is " + curriculumId)
          );
      }

      public void saveCurriculum(Curriculum curriculum) {
          curriculumRepository.save(curriculum);
      }

      @Transactional(readOnly = true)
      public List<Curriculum> findAllCurriculum() {
          return curriculumRepository.findAll();
      }

      @Transactional(readOnly = true)
      public List<Curriculum> findCurriculumListByCurriculumIds(List<Long> curriculumIds) {
          return curriculumRepository.findAllById(curriculumIds);
      }

      public void deleteCurriculum(Curriculum curriculum) {
          curriculumRepository.delete(curriculum);
      }

      public List<Curriculum> findCurriculumListByCourseId(Long courseId){
          return curriculumRepository.findAllByCourseId(courseId);
      }

      // MemberRepository
      @Transactional(readOnly = true)
      public Member findMemberByMemberId(Long memberId) {
          return memberRepository.findById(memberId).orElseThrow(
                  () -> new NotFoundException("Not Found Member : MemberId is " + memberId)
          );
      }

      // InstructorRepository
      @Transactional(readOnly = true)
      public Instructor findInstructorByInstructorId(Long instructorId) {
          return instructorRepository.findById(instructorId).orElseThrow(
                  () -> new NotFoundException("Not Found Instructor : InstructorId is " + instructorId)
          );
      }

      // TagRepository
      @Transactional(readOnly = true)
      public Tag findTagByTagId(Long tagId) {
          return tagRepository.findById(tagId).orElseThrow(
                  () -> new NotFoundException("Not Found Tag : TagId is " + tagId)
          );
      }

      @Transactional(readOnly = true)
      public List<Tag> findTagListByTagIds(List<Long> tagIds) {
          return tagRepository.findAllById(tagIds);
      }

      @Transactional(readOnly = true)
      public Long getTagCount() {
          return tagRepository.count();
      }

      // FavorRepository
      public void saveFavor(Favor favor) {
          favorRepository.save(favor);
      }

      public void deleteFavor(Favor favor) {
          favorRepository.delete(favor);
      }

      @Transactional(readOnly = true)
      public List<Favor> findAllFavorByMemberId(Long memberId) {
          return favorRepository.findByMemberId(memberId);
      }

      // ReviewRepository
      public void saveReview(Review review) {
          reviewRepository.save(review);
      }

      public void deleteReview(Review review) {
          reviewRepository.delete(review);
      }

      public List<Review> findAllReviewByCourseId(Long courseId) {
          return reviewRepository.findByCourseId(courseId);
      }

      public Review findReviewByReviewId(Long reviewId) {
          return reviewRepository.findById(reviewId).orElseThrow(
                  () -> new NotFoundException("Not Found Review : ReviewId is " + reviewId)
          );
      }
  }

  ```
- `AppliedService`
  ```java
  @Service
  @RequiredArgsConstructor
  @Transactional
  public class AppliedService {

      private final FavorRepository favorRepository;
      private final InstructorRepository instructorRepository;
      private final RegisterRepository registerRepository;
      private final ReviewRepository reviewRepository;
      private final TagRepository tagRepository;

      @Transactional(readOnly = true)
      public List<TagRes> getAllTagres() {
          return tagRepository.getTags();
      }

      @Transactional(readOnly = true)
      public Long getRegisterCount(Long courseId) {
          return registerRepository.countByCourseId(courseId);
      }

      @Transactional(readOnly = true)
      public InstructorRes getInstructorByCourseId(Long courseId) {
          return instructorRepository.findInstructorByCourseId(courseId).orElseThrow(
                  () -> new NotFoundException("Not Found Instructor of Course : Course_id is " + courseId)
          );
      }

      @Transactional(readOnly = true)
       public Instructor getInstructorIdByCourseId(Long courseId) {
          return instructorRepository.findInstructorIdByCourseId(courseId).orElseThrow(
                  () -> new NotFoundException("Not Found Instructor of Course : Course_id is " + courseId)
          );
      }

      // CourseRepository
      @Transactional(readOnly = true)
      public Long checkRegisterStatus(Long memberId, Long courseId) {
          return registerRepository.checkRegisterStatus(memberId, courseId);
      }

      // ReviewRepository
      @Transactional(readOnly = true)
      public boolean findReviewByMemberIdAndCourseId(Long memberId, Long courseId) {
          return reviewRepository.findByMemberIdAndCourseId(memberId, courseId).isPresent();
      }
  }

  ```

`JpaRepository`에 기본으로 구현되어 있는 메소드의 경우 `BasicService`로, JPQL을 이용해 직접 구현한 메소드의 경우 `AppliedService`로 감쌌다. 이를 통해 서비스 클래스간의 **순환 참조**를 해결하고, 한 개의 서비스 계층이 여러 개의 데이터 접근 계층에 의존하는 문제를 해결했다.

<aside>
💡 **순환참조란?**

[순환 참조](https://www.notion.so/746442d787a54168805bd55ae9275cbb?pvs=21)

</aside>
