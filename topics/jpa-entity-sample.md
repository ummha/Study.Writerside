# 엔티티 기본 작성 양식

```java
@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "SAMPLE_MANAGEMENT")
@Comment("샘플 관리")
@EntityListeners({AuditingListener.class})
public class SampleManagement implements Auditing.Create, Auditing.Update {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, 
									generator = "SampleManagementSeq")
  @SequenceGenerator(name = "SampleManagementSeq", 
										 sequenceName = "SAMPLE_MANAGEMENT_SEQ", 
										 allocationSize = 1)
  @Column(name = "ID", columnDefinition = "NUMBER(19,0)")
	@Comment("샘플 관리 ID")
  private Long id;

  @Size(max = 20)
  @Column(name = "SAMPLE_CODE", columnDefinition = "VARCHAR2(20)")
  @Comment("샘플 코드")
  private String sampleCode;

  @Size(max = 50)
  @Nationalized
  @Column(name = "TITLE", columnDefinition = "NVARCHAR2(50)")
  @Comment("샘플 제목")
  private String title;

	@Lob
  @Column(name = "CONTENT", columnDefinition = "CLOB")
  @Comment("샘플 내용")
  private String content;

  @ColumnDefault("1")
  @Convert(converter = BooleanConverter.class)
  @Column(name = "IS_USED", columnDefinition = "NUMBER(1)")
  @Comment("사용 여부")
	private boolean isUsed = true;

	@Embedded
	private CreateAudit createAudit;

	@Embedded
	private UpdateAudit updateAudit;
}
```