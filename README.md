# formangular

export class FormComponent implements OnInit {

  form: FormGroup;

  constructor() {
    this.buildForm();
  }

  ngOnInit() {
  }

  private buildForm() {
    this.form = new FormGroup({
      name: new FormControl('', [Validators.required]),
      date: new FormControl('', [Validators.required]),
      email: new FormControl('', [Validators.email]),
      text: new FormControl('', [Validators.maxLength(200)]),
      category: new FormControl('', [Validators.required]),
      gender: new FormControl('', [Validators.required]),
    });

    this.form.valueChanges
    .subscribe(value => {
      console.log(value);
    });
  }

}
2. Create html
<form [formGroup]="form">
  <p>
    Name: <br/>
    <input type="text" formControlName="name">
  </p>
  <p>
    Fecha: <br/>
    <input type="date" formControlName="date">
  </p>
  <p>
    Email: <br/>
    <input type="email" formControlName="email">
  </p>
  <p>
    Texto largo: <br/>
    <textarea cols="30" rows="10" formControlName="text"></textarea>
  </p>
  <p>
    Category: <br/>
    <select formControlName="category">
      <option value="1">Category 1</option>
      <option value="2">Category 2</option>
      <option value="3">Category 3</option>
    </select>
  </p>
  <p>
    Genero: <br/>
    <input type="radio" name="gender" formControlName="gender" value="male"> Male<br>
    <input type="radio" name="gender" formControlName="gender" value="female"> Female<br>
    <input type="radio" name="gender" formControlName="gender" value="other"> Other
  </p>
</form>
3.
<form [formGroup]="form" (ngSubmit)="save($event)">
  ...
  <button type="submit">Guardar</button> 
</form>
save(event: Event) {
  event.preventDefault();
  const value = this.form.value;
  console.log(value);
}
4. FormBuilder
this.form = this.formBuilder.group({
  name: ['',  [Validators.required]],
  date: ['', [Validators.required]],
  email: ['', [Validators.required, Validators.email]],
  text: ['', [Validators.required, Validators.maxLength(200)]],
  category: ['', [Validators.required]],
  gender: ['', [Validators.required]],
});
5. Validations
<form [formGroup]="form" (ngSubmit)="save($event)">
  ...
  <button type="submit" [disabled]="form.invalid">Guardar</button> 
</form>
save(event: Event) {
  event.preventDefault();
  if (this.form.valid) {
    const value = this.form.value;
    console.log(value);
  }
}
5. Validations by Field
<p>
    Texto largo: <br/>
  <textarea cols="30" rows="10" formControlName="text"></textarea>
  <small>{{ this.form.get('text').value.length }}/80</small>
</p>
<div *ngIf="form.get('text').errors && form.get('text').touched">
  <p *ngIf="form.get('text').hasError('required')">
    Es un campo requerido
  </p>
  <p *ngIf="form.get('text').hasError('maxlength')">
    Te pasaste, máximo deben haber 80 caracteres.
  </p>
</div>
save(event: Event) {
  event.preventDefault();
  if (this.form.valid) {
    const value = this.form.value;
    console.log(value);
  } else {
    this.form.markAllAsTouched();
  }
}
7. Clean code
<p>
  Texto largo: <br/>
  <textarea cols="30" rows="10" formControlName="text"></textarea>
  <small>{{ textField.value.length }}/80</small>
</p>
<div *ngIf="textField.errors && textField.touched">
  <p *ngIf="textField.hasError('required')">
    Es un campo requerido
  </p>
  <p *ngIf="textField.hasError('maxlength')">
    Te pasaste, máximo deben haber 80 caracteres.
  </p>
</div>
this.textField.valueChanges
.pipe(
  debounceTime(500)
)
.subscribe(value => {
  console.log(value);
});
6. Extras
<form [formGroup]="form" novalidate (ngSubmit)="save($event)">
  ...
  <button (click)="doSomething()">Action</button> 
  <button type="submit" [disabled]="form.invalid">Guardar</button> 
</form>
doSomething() {
  console.log('click');
}
